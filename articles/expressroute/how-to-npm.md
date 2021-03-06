---
title: "为 Azure ExpressRoute 线路配置网络性能监视器（预览版）| Microsoft Docs"
description: "为 Azure ExpressRoute 线路配置 NPM。 （预览版）"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: cherylmc
ms.openlocfilehash: 3ab8029d035c3ba88ddb8a112e27f9054f7c203c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="configure-network-performance-monitor-for-expressroute-preview"></a>为 ExpressRoute 配置网络性能监视器（预览版）

网络性能监视器 (NPM) 是基于云的网络监视解决方案，用于监视 Azure 云部署和本地位置（分支机构等）之间的连接。 NPM 属于 Microsoft Operations Management Suite (OMS)。 NPM 现在可为 ExpressRoute 提供扩展，使你能通过配置为使用专用对等互连的 ExpressRoute 线路监视网络性能。 为 ExpressRoute 配置 NPM 后，可以检测到需要识别和消除的网络问题。

可以：

* 跨多种 VNet 监视数据丢失和延迟情况并设置警报

* 监视网络上的所有路径（包括冗余路径）

* 对难以复制且在特定时间点出现的暂时性网络问题进行故障排除

* 帮助确定网络上导致性能下降的特定分段

* 获取每个虚拟网络的吞吐量（如果已在每个 VNet 中安装代理）

* 查看之前某一时间的 ExpressRoute 系统状态

## <a name="regions"></a>支持的区域

通过使用以下任一区域托管的工作区，可以在世界上任何地方监视 ExpressRoute 线路：

* 欧洲西部 
* 美国东部 
* 东南亚 

## <a name="workflow"></a>工作流

监视本地和 Azure 中的多个服务器上安装的代理。 代理相互通信，但不会发送数据，而是发送 TCP 握手数据包。 通过代理间的通信，Azure 可以映射流量可能经过的网络拓扑和路径。

1. 在其中一个[受支持区域](#regions)创建 NPM 工作区。
2. 安装和配置软件代理： 
    * 在本地服务器和 Azure VM 上安装监视代理。
    * 在监视代理服务器上配置设置，允许监视代理进行通信。 （打开防火墙端口等）
3. 配置网络安全组 (NSG) 规则，允许 Azure VM 上安装的监视代理与本地监视代理进行通信。
4. 请求将 NPM 工作区加入白名单。
5. 设置监视：自动发现和管理在 NPM 中可见的网络。

如果已使用网络性能监视器监视其他对象或服务，并且在一个受支持区域已有工作区，则可以跳过步骤 1 和步骤 2，从步骤 3 开始配置。

## <a name="configure"></a>步骤 1：创建工作区

1. 在 [Azure 门户](https://portal.azure.com)中，从 Marketplace 服务列表中搜索“网络性能监视器”。 在返回结果中，单击打开“网络性能监视器”页面。

  ![portal](.\media\how-to-npm\3.png)<br><br>
2. 在“网络性能监视器”主页底部，单击“创建”打开“网络性能监视器 - 创建新解决方案”页面。 单击“OMS 工作区 - 选择工作区”打开“工作区”页面。 单击“+ 创建新工作区”打开“工作区”页面。
3. 在“OMS 工作区”页面上，选择“新建”并配置下列设置：

  * “OMS 工作区”- 键入工作区的名称。
  * “订阅”- 若有多个订阅，请选择要与新工作区相关联的订阅。
  * “资源组”- 创建一个资源组或使用现有资源组。
  * 位置 - 必须选择一个[受支持区域](#regions)。
  * “定价层”- 选择“免费”
  
  >[!NOTE]
  >ExpressRoute 线路可以位于世界上任意位置，并且无需与工作区在同一区域。
  >


  ![工作区](.\media\how-to-npm\4.png)<br><br>
4. 单击“确定”保存和部署设置模板。 模板验证后，单击“创建”以部署工作区。
5. 部署工作区后，导航到创建的“NetworkMonitoring(名称)”资源。 验证设置，然后单击“解决方案需要其他配置”。

  ![其他配置](.\media\how-to-npm\5.png)
6. 在“欢迎使用网络性能监视器”页面，选择“将 TCP 用于综合事务”，然后单击“提交”。 TCP 事务仅用于建立和断开连接。 不会通过这些 TCP 发送任何数据。

  ![用于综合事务的 TCP](.\media\how-to-npm\6.png)

## <a name="agents"></a>步骤 2：安装和配置代理

### <a name="download"></a>2.1：下载代理安装程序文件

1. 在资源的“网络性能监视器配置 - TCP 安装”页面上的“安装 OMS 代理”部分，单击与你的服务器处理器的对应代理，然后下载安装程序文件。

  >[!NOTE]
  >目前不支持 Linux 代理进行 ExpressRoute 监视。
  >
  >
2. 接下来，将“工作区 ID”和“主密钥”复制到记事本。
3. 在“配置代理”部分，下载 Powershell 脚本。 PowerShell 脚本可帮助你打开与 TCP 事务相关的防火墙端口。

  ![PowerShell 脚本](.\media\how-to-npm\7.png)

### <a name="installagent"></a>2.2：在每个监视服务器上安装监视代理

1. 运行安装程序，在要用于监视 ExpressRoute 的每个服务器上安装代理。 用于监视的服务器可以是 VM 或本地服务器，并且必须连接 Internet。 需要至少在本地安装一个代理，并在 Azure 中在要监视的每个网络段上安装一个代理。
2. 在“欢迎”页面上，单击“下一步”。
3. 在“许可条款”页面上阅读许可协议，然后单击“我接受”。
4. 在“目标文件夹”页面上更改或保留默认安装文件夹，然后单击“下一步”。
5. 在“代理安装选项”页上，可以选择将代理连接到 Azure Log Analytics (OMS) 或 Operations Manager。 或者，如果希望稍后配置代理，也可以将选项留空。 完成选择后，单击“下一步”。

  * 如果选择连接到 Azure Log Analytics (OMS)，请粘贴在前面步骤中复制到记事本的“工作区 ID” 和“工作区密钥”（主密钥）。 然后单击“下一步”。

    ![ID 和密钥](.\media\how-to-npm\8.png)
  * 如果选择连接到 Operations Manager，请在“管理组配置”页面键入“管理组名称”、“管理服务器”和“管理服务器端口”。 然后单击“下一步”。

    ![Operations Manager](.\media\how-to-npm\9.png)
  * 在“代理操作帐户”页面，选择“本地系统”帐户或“域或本地计算机帐户”。 然后单击“下一步”。

    ![帐户](.\media\how-to-npm\10.png)
6. 在“准备安装”页上检查所做的选择，并单击“安装”。
7. 在“配置已成功完成”页上，单击“完成”。
8. 完成后，Microsoft Monitoring Agent 将显示在“控制面板”中。 可在该处查看配置并验证代理是否已连接到操作见解 (OMS)。 如果连接到 OMS，代理会显示一条消息，指出：“Microsoft Monitoring Agent 已成功连接到 Microsoft Operations Management Suite 服务”。

### <a name="proxy"></a>2.3：配置代理设置（可选）

如果要使用 Web 代理访问 Internet，请执行以下步骤为 Microsoft Monitoring Agent 配置代理设置。 针对每个服务器执行这些步骤。 如果需要配置多台服务器，使用脚本自动执行此过程可能更加轻松。 如果是此情况，请参阅[使用脚本为 Microsoft Monitoring Agent 配置代理设置](../log-analytics/log-analytics-windows-agents.md#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script)。

使用控制面板为 Microsoft Monitoring Agent 配置代理设置：

1. 打开“控制面板”。
2. 打开“Microsoft Monitoring Agent”
3. 单击“代理设置”选项卡。
4. 选择“使用代理服务器”并键入 URL 和端口号（如果需要）。 如果代理服务器要求身份验证，请键入用户名和密码以访问代理服务器。

  ![proxy](.\media\how-to-npm\11.png)

### <a name="verifyagent"></a>2.4：验证代理的连接

可以轻松验证代理是否正在通信。

1. 在具有监视代理的服务器上，打开“控制面板”。
2. 打开“Microsoft Monitoring Agent”。
3. 单击“Azure Log Analytics (OMS)”选项卡。
4. 在“状态”列中，应该会看到代理成功连接到 Operations Management Suite 服务。

  ![status](.\media\how-to-npm\12.png)

### <a name="firewall"></a>2.5：打开监视代理服务器上的防火墙端口

若要使用 TCP 协议，必须打开防火墙端口以确保监视代理可以通信。

可以运行 PowerShell 脚本，该脚本会创建网络性能监视器所需的注册表项，还会创建防火墙规则以允许监视代理彼此创建 TCP 连接。 该脚本创建的注册表项还指定是否记录调试日志和该日志文件的路径。 还会定义用于通信的代理 TCP 端口。 该脚本会自动设置这些注册表项的值，因此不应手动更改这些注册表项。

8084 端口默认打开。 通过向该脚本提供参数“portNumber”即可使用自定义端口。 但是，如果这样做，则必须为运行脚本的所有服务器指定同一端口。

>[!NOTE]
>“EnableRules”PowerShell 脚本仅在运行该脚本的服务器上配置 Windows 防火墙规则。 如果有网络防火墙，应确保该防火墙允许流量去往网络性能监视器正在使用的 TCP 端口。
>
>

在代理服务器上，使用管理权限打开 PowerShell 窗口。 运行 [EnableRules](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) PowerShell 脚本（之前已下载）。 不要使用任何参数。

  ![PowerShell_Script](.\media\how-to-npm\script.png)

## <a name="opennsg"></a>步骤 3：配置网络安全组规则

对于 Azure 中的监视代理服务器，必须配置网络安全组 (NSG) 规则，允许将 NPM 在端口上使用的 TCP 流量用于综合事务。 默认端口为 8084。 通过此操作，Azure VM 上安装的监视代理将可与本地监视代理进行通信。

有关 NSG 的详细信息，请参阅[网络安全组](../virtual-network/virtual-networks-create-nsg-arm-portal.md)。

## <a name="whitelist"></a>步骤 4：请求将工作区加入白名单

>[!NOTE]
>请确保已安装代理（包括本地服务器代理和 Azure 服务器代理），并且在执行此步骤前已运行 PowerShell 脚本。
>
>

若要开始使用 NPM 的 ExpressRoute 监视功能，必须先请求将你的工作区加入白名单。 [单击此处转到对应页面并填写请求窗体](https://aka.ms/npmcohort)。 （提示：可能需要在新窗口或新选项卡中打开此链接。） 加入白名单的过程可能至少需要一个工作日。 加入白名单完成后，将收到一封电子邮件。

## <a name="setupmonitor"></a>步骤 5：配置 NPM 进行 ExpressRoute 监视

>[!WARNING]
>若要继续执行操作，工作区必须已加入白名单并且已收到确认电子邮件。
>
>

完成之前的部分并验证已加入白名单后，便可以设置监视功能。

1. 转到“所有资源”页面，单击已加入白名单的 NPM 工作区，导航到“网络性能监视器”概述磁贴。

  ![npm 工作区](.\media\how-to-npm\npm.png)
2. 单击“网络性能监视器概述”磁贴，调出仪表板。 仪表板包含一个 ExpressRoute 页面，其中显示 ExpressRoute 处于“未配置状态”。 单击“功能设置”，打开“网络性能监视器”配置页。

  ![功能设置](.\media\how-to-npm\npm2.png)
3. 在配置页面，导航到左侧面板上的“ExpressRoute 对等互连”选项卡。 单击“立刻发现”。

  ![发现](.\media\how-to-npm\13.png)
4. 完成发现后，可以查看唯一线路名称和 VNet 名称的规则。 这些规则起初是禁用的。 请启用规则，然后选择监视代理和阈值。

  ![规则](.\media\how-to-npm\14.png)
5. 启用规则并选择要监视的的值和代理后，需要等待约 30-60 分钟后值才会开始填充，“ExpressRoute 监视”磁贴也将变为可用。 看到监视磁铁后，NPM 也将开始监视 ExpressRoute 线路和连接资源。

  ![监视磁贴](.\media\how-to-npm\15.png)

## <a name="explore"></a>步骤 6：查看监视磁贴

### <a name="dashboard"></a>网络性能监视器页面

NPM 页面包含一个 ExpressRoute 页面，其中显示 ExpressRoute 线路和对等互连的运行状况。

  ![仪表板](.\media\how-to-npm\dashboard.png)

### <a name="circuits"></a>线路列表

若要查看所有受监视的 ExpressRoute 线路的列表，请单击“ExpressRoute 线路”磁贴。 可以选择一条线路并查看其运行状态以及数据包丢失、带宽使用率和延迟的趋势图表。 这些图表是交互式的。 可以选择自定义时间段来绘制图表。 可以在图表上方区域拖动鼠标来放大图表，详细查看数据点。

  ![circuit_list](.\media\how-to-npm\circuits.png)

#### <a name="trend"></a>丢失、延迟和吞吐量的趋势

带宽、延迟和丢失图表是交互式的。 可以使用鼠标控件放大这些图表的任何部分。 单击左上角“操作”按钮下的“日期/时间”，还可以查看其它时间间隔内的带宽、延迟和数据丢失趋势。

![趋势](.\media\how-to-npm\16.png)

### <a name="peerings"></a>对等互连列表

单击仪表板上的“专用对等互连”磁贴，会显示通过专用对等互连到虚拟网络的所有连接列表。 可以在此处选择一个虚拟网络连接，并查看其运行状态以及数据包丢失、带宽使用率和延迟的趋势图表。

  ![线路列表](.\media\how-to-npm\peerings.png)

### <a name="topology"></a>线路拓扑

若要查看线路拓扑，请单击“拓扑”磁贴。 你将转到所选线路或对等互连的拓扑视图。 此拓扑图显示该网络上每个分段的延迟情况，并且每个第 3 层跃点由图表上的一个节点表示。 单击跃点即可查看该跃点的详细信息。
通过移动“筛选器”下的滚动条，可以增加可见级别以包含本地跃点。 向左/右移动滚动条即可增加/减少拓扑图中的跃点数。 还将显示每个分段的延迟情况，据此可以更快地隔离网络中的高延迟分段。

![筛选器](.\media\how-to-npm\topology.png)

#### <a name="detailed-topology-view-of-a-circuit"></a>线路的详细拓扑图视图

此视图显示 VNet 连接。
![详细拓扑图](.\media\how-to-npm\17.png)
