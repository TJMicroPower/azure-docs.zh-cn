---
title: "Azure CLI 脚本示例 - 使用 OMS monitoring 创建 Windows Server 2016 VM | Microsoft 文档"
description: "Azure CLI 脚本示例 - 使用 OMS monitoring 创建 Windows Server 2016 VM"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
translationtype: Human Translation
ms.sourcegitcommit: 82d40c30c92f5da090e7ec4e2f25ead3908cc603
ms.openlocfilehash: ade388f38b7c1f11668c7a356868c9cacd3386f0
ms.lasthandoff: 03/02/2017

---

# <a name="monitor-a-vm-with-operations-management-suite"></a>使用 Operations Management Suite 监视 VM

此脚本创建一个 Azure 虚拟机，安装 Operations Management Suite (OMS) 代理，并将系统注册到 OMS 工作区。 运行脚本后，该虚拟机将显示在 OMS 控制台中。

在运行此脚本前，请确保已使用 `az login` 命令创建与 Azure 的连接。 此外，应将该脚本开头的 $AdminPassword 变量更改为唯一和满足密码复杂性要求。

此示例在 Bash Shell 中正常工作。 有关在 Windows 上运行 Azure CLI 脚本的选项，请参阅[在 Windows 中运行 Azure CLI](../virtual-machines-windows-cli-options.md)。

## <a name="sample-script"></a>示例脚本

[!code-azurecli[主要](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-monitor-oms.sh "快速创建 VM")]

## <a name="clean-up-deployment"></a>清理部署 

运行脚本示例后，可以使用以下命令删除资源组、VM 以及所有相关资源。

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和 NSG。 此命令还指定要使用的虚拟机映像和管理凭据。  |
| [azure vm extension set](https://docs.microsoft.com/cli/azure/vm/extension#set) | 针对虚拟机运行 VM 扩展。 在此示例中，使用 Operations Management Suite 代理扩展安装 OMS 代理，并在 OMS 工作区中注册 VM。 |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Windows VM 文档](../virtual-machines-windows-cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。
