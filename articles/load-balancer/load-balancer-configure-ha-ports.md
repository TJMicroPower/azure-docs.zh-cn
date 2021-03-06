---
title: "为 Azure 负载均衡器配置高可用性端口 | Microsoft Docs"
description: "了解如何使用高可用性端口对所有端口上的内部流量进行负载均衡"
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/02/2017
ms.author: kumud
ms.openlocfilehash: 4cd65c01d75af8539f5fa13dbbd2aaec548aea0b
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-configure-high-availability-ports-for-internal-load-balancer"></a>如何为内部负载均衡器配置高可用性端口

本文提供了在内部负载均衡器上部署高可用性 (HA) 端口的示例。 有关特定于网络虚拟设备 (NVA) 的配置，请参阅相应的提供程序网站。

>[!NOTE]
> 高可用性端口功能当前处于预览状态。 在预览期，该功能的可用性和可靠性级别可能与正式版不同。 有关详细信息，请参阅 [Microsoft Azure 预览版 Microsoft Azure 补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

图 1 演示了本文中所述部署示例的以下配置：
- NVA 部署在 HA 端口配置后面内部负载均衡器的后端池中。 
- 应用于外围网络子网的 UDR 通过将下一跃点设为内部负载均衡器虚拟 IP 将所有流量路由到 NVA。 
- 内部负载均衡器根据 LB 算法将流量分配到某个活动 NVA。
- NVA 处理流量并将其转发到后端子网中的原始目标。
- 如果在后端子网中配置了相应 UDR，返回路径也可以采用相同的路由。 

![ha 端口部署示例](./media/load-balancer-configure-ha-ports/haports.png)

图 1 - 部署在内部负载均衡器后面，具有高可用性端口的网络虚拟设备 

## <a name="preview-sign-up"></a>预览版注册

若要体验负载均衡器标准版中 HA 端口功能的预览版，请使用 Azure CLI 2.0 或 PowerShell 注册订阅，以获取访问权限。  可以注册订阅以获取

1. [负载均衡器标准预览版](https://aka.ms/lbpreview#preview-sign-up)和 
2. [HA 端口预览版](https://aka.ms/haports#preview-sign-up)。

>[!NOTE]
>要使用此功能，除了注册 HA 端口外，还需注册负载均衡器[标准版（预览版）](https://aka.ms/lbpreview#preview-sign-up)。 注册 HA 端口或负载均衡器标准版（预览版）最长可能需要一小时。

## <a name="configuring-ha-ports"></a>配置 HA 端口

HA 端口的配置涉及使用后端池中的 NVA 设置内部负载均衡器，设置用于检测 NVA 运行状况的相应负载均衡器运行状况探测配置，以及设置包含 HA 端口的负载均衡器规则。 [入门](load-balancer-get-started-ilb-arm-portal.md)中介绍了常规的负载均衡器相关配置。 本文重点介绍 HA 端口配置。

该配置实质上包括将前端端口和后端端口值设置为 **0**，将协议值设置为 **All**。 本文介绍如何使用 Azure 门户、PowerShell 和 Azure CLI 2.0 配置高可用性端口。

### <a name="configure-ha-ports-load-balancer-rule-with-the-azure-portal"></a>使用 Azure 门户配置 HA 端口负载均衡器规则

Azure 门户为此配置提供“HA 端口”选项（通过复选框实现）。 选中时，会自动填充相关端口和协议配置。 

![通过 Azure 门户进行 ha 端口配置](./media/load-balancer-configure-ha-ports/haports-portal.png)

图 2 - 通过门户进行 HA 端口配置

### <a name="configure-ha-ports-lb-rule-via-resource-manager-template"></a>通过资源管理器模板配置 HA 端口负载均衡器规则

可以使用 2017-08-01 API 版本为负载均衡器资源中的 Microsoft.Network/loadBalancers 配置 HA 端口。 以下 JSON 代码片段演示通过 REST API 配置的 HA 端口的负载均衡器配置中的更改。

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-ha-ports-load-balancer-rule-with-powershell"></a>使用 PowerShell 配置 HA 端口负载均衡器规则

使用以下命令，在使用 PowerShell 创建内部负载均衡器时，创建 HA 端口负载均衡器规则：

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-ha-ports-load-balancer-rule-with-azure-cli-20"></a>使用 Azure CLI 2.0 配置 HA 端口负载均衡器规则

在[创建内部负载均衡器集](load-balancer-get-started-ilb-arm-cli.md)的步骤 4，使用以下命令创建 HA 端口负载均衡器规则。

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>后续步骤

- 详细了解[高可用性端口](load-balancer-ha-ports-overview.md)
