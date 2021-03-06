---
title: "分析 Azure CDN 使用模式 |Microsoft Docs"
description: "客户可为 Azure CDN 启用日志分析。"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: 04e5499011e72dfcc20dff370d5d837227ed29b6
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>分析 Azure CDN 使用模式

为应用程序启用 CDN 后，可以监视 CDN 使用情况、检查交付内容的运行状况以及排除潜在问题。 Azure CDN 通过以下两种方式提供这些功能： 

## <a name="verizon-core-reports"></a>Verizon 核心报表

作为具有 Verizon 标准版或 Verizon 高级版配置文件的 Azure CDN 用户，可以在 Verizon 补充门户中查看 Verizon 核心报表。 可通过 Azure 门户的“管理”选项访问 Verizon 核心报表，其中提供了各种各样的图和视图。 有关详细信息，请参阅 [Verizon 中的核心报表](cdn-analyze-usage-patterns.md)。

## <a name="verizon-custom-reports"></a>Verizon 自定义报表

作为具有 Verizon 标准版或 Verizon 高级版配置文件的 Azure CDN 用户，可以在 Verizon 补充门户中查看 Verizon 自定义报表。 可通过 Azure 门户的“管理”选项访问 Verizon 自定义报表。 Verizon 自定义报表页显示属于 Azure CDN 配置文件的各个边缘 CName 的点击量或传输数据。 可以按照任意时间段内的 HTTP 响应代码或缓存状态将数据分组。 有关详细信息，请参阅 [Verizon 中的自定义报表](cdn-verizon-custom-reports.md)。

## <a name="core-analytics-via-azure-diagnostic-logs"></a>通过 Azure 诊断日志进行核心分析

核心分析适用于所有属于 Verizon（标准版和高级版）和 Akamai（标准版）CDN 配置文件的 CDN 终结点。 Azure 诊断日志使核心分析可以导出到 Azure 存储、事件中心或 Operations Management Suite (OMS) Log Analytics。 OMS Log Analytics 提供的解决方案中具有用户可配置且可自定义的图。 有关详细信息，请参阅 [Azure 诊断日志](cdn-azure-diagnostic-logs.md)。

