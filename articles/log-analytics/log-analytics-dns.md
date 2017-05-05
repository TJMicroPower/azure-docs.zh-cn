---
title: "Azure Log Analytics 中的 DNS Analytics 解决方案 | Microsoft Docs"
description: "在 Log Analytics 中设置并使用 DNS Analytics 解决方案，收集有关 DNS 基础结构安全性、性能和操作的见解。"
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f44a40c4-820a-406e-8c40-70bd8dc67ae7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 4473ca68374b96f10c60282135e83f7ec390bb48
ms.lasthandoff: 04/20/2017


---

# <a name="gather-insights-about-your-dns-infrastructure-with-the-dns-analytics-preview-solution"></a>使用 DNS Analytics（预览版）解决方案收集有关 DNS 基础结构的见解

本文介绍如何在 Log Analytics 中设置并使用 DNS Analytics 解决方案，收集有关 DNS 基础结构安全性、性能和操作的见解。

DNS Analytics 可帮助：

- 确定尝试解决恶意域名的客户端
- 确定过时的资源记录
- 确定经常查询的域名和请求频繁的 DNS 客户端
- 查看 DNS 服务器上的请求负载
- 查看动态 DNS 注册错误

解决方案收集、分析和关联 Windows DNS 分析和审核日志，以及 DNS 服务器的其他相关数据。

## <a name="connected-sources"></a>连接的源

下表介绍了该解决方案支持的连接的源。

| **连接的源** | **支持** | **说明** |
| --- | --- | --- |
| [Windows 代理](log-analytics-windows-agents.md) | 是 | 解决方案会从 Windows 代理收集 DNS 信息。 |
| [Linux 代理](log-analytics-linux-agents.md) | 否 | 解决方案不会从直接 Linux 代理收集 DNS 信息。 |
| [SCOM 管理组](log-analytics-om-agents.md) | 是 | 解决方案会从连接的 SCOM 管理组中的代理收集 DNS 信息。 不需要从 SCOM 代理直接连接到 OMS。 数据从管理组转发到 OMS 存储库。 |
| [Azure 存储帐户](log-analytics-azure-storage.md) | 否 | 解决方案不会使用 Azure 存储。 |

### <a name="data-collection-details"></a>数据收集详细信息

解决方案从安装有 Log Analytics 代理的 DNS 服务器收集 DNS 清单以及与 DNS 事件相关的数据。 此数据稍后将上传到 Log Analytics，之后将显示在解决方案仪表板中。 通过运行 DNS Powershell cmdlet 收集与清单相关的数据，如 DNS 服务器的数量、区域和资源记录。 该数据每两天更新一次。 与事件相关的数据几乎是从由 Windows Server 2012 R2 中增强的 DNS 日志记录和诊断提供的[分析和审核日志](https://technet.microsoft.com/library/dn800669.aspx#enhanc)中实时收集的。

## <a name="configuration"></a>配置

使用以下信息配置解决方案。

- 在要监视的每个 DNS 服务器上，都必须装有 [Windows](log-analytics-windows-agents.md) 或 [Operations Manager](log-analytics-om-agents.md) 代理。
- 从 [Azure 应用商店](https://aka.ms/dnsanalyticsazuremarketplace)或使用[从解决方案库中添加 Log Analytics 解决方案](log-analytics-add-solutions.md)中所述的过程，将 DNS Analytics 解决方案添加到 OMS 工作区。

解决方案将开始收集数据，而无需进一步配置。 但是，可使用以下配置自定义数据收集。

###  <a name="configure-the-solution"></a>配置解决方案

在解决方案仪表板上，单击“配置”打开 DNS Analytics 配置页面。 可进行两种类型的配置更改：

- 将域名列入允许列表：解决方案不会处理所有查找查询。 这样可保留域名后缀允许列表。 查找查询会解析为匹配此允许列表中域名后缀的域名，但不由解决方案处理。 不处理列入允许列表的域名有助于优化发送到 Log Analytics 的数据。 默认允许列表包括常用的公共域名，例如 www.google.com 和 www.facebook.com。 使用滚动条可查看完整的默认列表。

可以修改列表，添加或删除任何不感兴趣的域名后缀，从而查看查找见解。

- 客户端请求阈值：DNS 客户端超出查找请求数的阈值时，将突出显示在“DNS 客户端”边栏选项卡中。 默认阈值为 1000。 可以编辑该阈值。

![列入允许列表的域名](./media/log-analytics-dns/dns-config.png)

## <a name="management-packs"></a>管理包

如果使用 Microsoft Monitoring Agent (MMA) 连接到 OMS 工作区，则已安装以下管理包。

- Microsoft DNS 数据收集器智能包 (Microsft.IntelligencePacks.Dns)

如果 SCOM 管理组已连接到 OMS 工作区，则添加该解决方案时会在 SCOM 中安装以下管理包。 无需对这些管理包进行任何配置或维护。

- Microsoft DNS 数据收集器智能包 (Microsft.IntelligencePacks.Dns)
- Microsoft System Center Advisor DNS Analytics 配置 (Microsoft.IntelligencePack.Dns.Configuration)

有关如何更新解决方案管理包的详细信息，请参阅[将 Operations Manager 连接到 Log Analytics](log-analytics-om-agents.md)。

## <a name="use-the-solution"></a>使用解决方案

本部分介绍了所有仪表板函数及其使用方法。

将 DNS Analytics 解决方案添加到工作区后，OMS 概述页面的“解决方案”磁贴将提供 DNS 基础结构的快速摘要。 包括收集数据的 DNS 服务器的数量，及过去 24 小时内客户端为解决恶意域而发出的请求数。 单击该磁贴时，会打开解决方案仪表板。

![“DNS Analytics”磁贴](./media/log-analytics-dns/dns-tile.png)

### <a name="solution-dashboard"></a>解决方案仪表板

解决方案仪表板显示解决方案各种功能的摘要信息。 还包括指向用于取证分析和诊断的详细视图的链接。 默认显示过去七天的数据。 可以使用日期-时间选择控件更改日期和时间范围，与下图类似。

![时间选择控件](./media/log-analytics-dns/dns-time.png)

解决方案仪表板显示以下边栏选项卡：

DNS 安全性 - 报告正在尝试与恶意域进行通信的 DNS 客户端。 通过使用 Microsoft 威胁情报馈送，DNS Analytics 可以检测正在尝试访问恶意域的客户端 IP。 在许多情况下，通过解决恶意软件的域名，恶意软件影响设备对恶意域&quot;命令和控制&quot;中心的&quot;拨号&quot;。

![“DNS 安全性”边栏选项卡](./media/log-analytics-dns/dns-security-blade.png)

单击列表中的客户端 IP 时，将打开日志搜索，显示相应查询的查找详细信息。 在以下示例中，DNS Analytics 检测到与 [IRCbot](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=Win32/IRCbot) 的通信已完成。

![显示 ircbot 的日志搜索结果](./media/log-analytics-dns/ircbot.png)

该信息可帮助确定：

- 启动通信的客户端 IP
- 解析为恶意 IP 的域名
- 解析域名得到的 IP 地址
- 恶意 IP 地址
- 问题严重性
- 将恶意 IP 列入阻止列表的原因
- 检测时间

查询的域 - 提供环境中的 DNS 客户端正在查询的最常见域名。 可以查看所有查询的域名的列表，并向下钻取日志搜索中特定域名的查找请求详细信息。

![“查询的域”边栏选项卡](./media/log-analytics-dns/domains-queried-blade.png)

DNS 客户端 - 报告所选时间段内查询数超出阈值的客户端。 可以查看所有 DNS 客户端的列表，以及这些客户端在日志搜索中进行的查询的详细信息。

![“DNS 客户端”边栏选项卡](./media/log-analytics-dns/dns-clients-blade.png)

动态 DNS 注册 - 报告名称注册失败。 突出显示地址[资源记录](https://en.wikipedia.org/wiki/List_of_DNS_record_types)（类型 A 和 AAAA）的所有注册失败以及提出注册请求的客户端 IP。 然后可以使用此信息查找注册失败的根本原因：

1. 为客户端正在尝试更新的名称，找到权威的区域。
2. 使用解决方案检查该区域的清单信息。
3. 确认该区域的动态更新已启用。
4. 检查该区域是否已针对安全动态更新进行配置。

![“动态 DNS 注册”边栏选项卡](./media/log-analytics-dns/dynamic-dns-reg-blade.png)

名称注册请求 - 上面的磁贴显示成功和失败的 DNS 动态更新请求计数的趋势。 下面的磁贴列出前 10 个向 DNS 服务器发送 DNS 更新请求失败的客户端（按失败的次数排列）。

![“名称注册请求”边栏选项卡 ](./media/log-analytics-dns/name-reg-req-blade.png)

示例 DNS Analytics 查询 - 包含直接提取原始分析数据的最常见搜索查询的列表。

![示例查询](./media/log-analytics-dns/queries.png)

可以基于这些查询创建用于生成自定义报表的查询。

- 服务器列表：此链接会打开 DNS 日志搜索页面，其中显示所有 DNS 服务器及其关联的 FQDN、域名、林名和服务器 IP 的列表。
- DNS 区域的列表：此链接会打开 DNS 日志搜索页面，其中显示所有 DNS 区域及其关联的区域名称、动态更新状态、名称服务器和 DNSSEC 签名状态的的列表。
- 未使用的资源记录：此链接会打开 DNS 日志搜索页面，并显示所有未使用/过时的资源记录的列表。 此列表包含资源记录名称、资源记录类型、关联的 DNS 服务器、记录生成时间和区域名称。 可使用此列表确定不再使用的 DNS 资源记录。 然后可以根据此信息进行操作，从 DNS 服务器中删除这些条目。
- DNS 服务器查询负载：此链接会打开 DNS 日志搜索页面，可在其中了解 DNS 服务器上的 DNS 负载，从而帮助规划服务器的容量。 可以移到“指标”选项卡，将视图更改为图形可视化效果。 此视图可帮助了解 DNS 负载在 DNS 服务器上的分布方式。 它显示每个服务器的 DNS 查询速率趋势。
    ![DNS 服务器查询日志搜索结果](./media/log-analytics-dns/dns-servers-query-load.png)  
- DNS 区域查询负载：此链接会打开 DNS 日志搜索页面，可在其中看到由该解决方案中托管的 DNS 服务器上所有区域的第二条统计信息的 DNS 区域查询。 单击“指标”选项卡，将视图从详细记录更改为结果的图形可视化效果。
- 配置事件：此链接会打开 DNS 日志搜索页面，可在其中看到所有 DNS 配置更改事件和相关消息。 然后可以按照事件时间、事件 ID、DNS 服务器或任务类别筛选这些事件。 这样有助于审核在特定时间对特定的 DNS 服务器所做的更改。
- DNS 分析日志：此链接会打开 DNS 日志搜索页面，可在其中看到所有由该解决方案托管的 DNS 服务器上的所有分析事件。 然后可以按照事件时间、事件 ID、DNS 服务器、进行查找查询的客户端 IP 和查询类型任务类别，对这些事件进行筛选。 DNS 服务器分析事件允许对 DNS 服务器进行活动跟踪。 每次服务器发送或接收 DNS 信息时，都将记录一个分析事件。

### <a name="dns-log-search"></a>DNS 日志搜索

你可以在“搜索”页上创建查询，然后在搜索时使用 Facet 控件筛选结果。 还可创建高级查询以转换、筛选和报告结果。 可通过执行以下查询进行启动。

在“搜索查询”字段中，键入 `Type=DnsEvents`，查看由该解决方案托管的 DNS 服务器生成的所有 DNS 事件。 结果中将列出与查找查询、动态注册和配置更改相关的所有事件的日志数据。

![dnsevents 日志搜索](./media/log-analytics-dns/log-search-dnsevents.png)  

- 若要查看查找查询的日志数据，在 LHS Facet 控件中以“LookUpQuery”作为子类型进行筛选。 将显示包含所选时间段内查找查询事件的列表/表。
- 若要查看动态注册的日志数据，在 LHS Facet 控件中以“DynamicRegistration”作为子类型进行筛选。 将显示包含所选时间段内所有动态注册事件的列表/表。
- 若要查看配置更改的日志数据，在 LHS Facet 控件中以“ConfigurationChange”作为子类型进行筛选。 将显示包含所选时间段内所有配置更改事件的列表/表。

在“搜索查询”字段中，键入 `Type=DnsInventory`，查看由该解决方案托管的 DNS 服务器中所有与 DNS 清单相关数据。 结果中列出 DNS 服务器的日志数据、DNS 区域和资源记录。
![dnsinventory 日志搜索](./media/log-analytics-dns/log-search-dnsinventory.png)

## <a name="feedback"></a>反馈

可通过以下几种途径提供反馈：

- UserVoice 发布有关 DNS Analytics 功能的建议。 请访问 [OMS UserVoice 页](https://aka.ms/dnsanalyticsuservoice)。
- 加入我们的队列   我们始终欢迎新客户加入我们的队列，提前访问新功能，并在今后帮助我们改进 DNS Analytics。 如有兴趣加入我们的队列，请填写[此快速调查](https://aka.ms/dnsanalyticssurvey)。

## <a name="next-steps"></a>后续步骤

- [搜索日志](log-analytics-log-searches.md)以查看详细的 DNS 日志记录。
