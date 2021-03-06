---
title: "使用转换转换 XML 数据 - Azure 逻辑应用 | Microsoft 文档"
description: "使用 Enterprise Integration SDK 创建转换或映射将 XML 数据在逻辑应用的格式之间转换"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: f09819a1bfd380cd826a478471e673b6d5ff9ee7
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a>企业集成与 XML 转换
## <a name="overview"></a>概述
企业集成转换连接器可将数据从一种格式转换为另一种格式。 例如，传入消息可能包含 YearMonthDay 格式的当前日期。 可以使用转换将日期格式重新设置为 MonthDayYear 格式。

## <a name="what-does-a-transform-do"></a>转换的作用是什么？
转换（也称为映射）由源 XML 架构（输入）和目标 XML 架构（输出）组成。 可以使用不同的内置函数帮助操作或控制数据，包括字符串操作、条件分配、算术表达式、日期时间格式化程序甚至是循环构造。

## <a name="how-to-create-a-transform"></a>如何创建转换？
可以使用 Visual Studio [企业集成 SDK](https://aka.ms/vsmapsandschemas) 创建转换/映射。 完成转换的创建和测试之后，需将转换上传到集成帐户中。 

## <a name="how-to-use-a-transform"></a>如何使用转换
将转换或映射上传到集成帐户中之后，可以使用它创建逻辑应用。 逻辑应用随后会在每次触发逻辑应用（以及存在需要进行转换的输入内容）时运行转换。

**下面是使用转换的步骤**：

### <a name="prerequisites"></a>先决条件

* 创建集成帐户并向其中添加映射  

现在已满足了先决条件，可以创建逻辑应用了：  

1. 创建逻辑应用并[将它链接到包含映射的集成帐户](../logic-apps/logic-apps-enterprise-integration-accounts.md "了解如何将集成帐户链接到逻辑应用")。
2. 将“请求”触发器添加到逻辑应用  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. 通过首先选择“添加操作”来添加“转换 XML”操作   
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. 在搜索框中输入“转换”一词，以便在所有操作中筛选出要使用的操作  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. 选择“转换 XML”操作   
6. 添加转换的 XML“内容”。 可使用在 HTTP 请求中收到的任何 XML 数据作为“内容”。 在此示例中，选择触发了逻辑应用的 HTTP 请求的正文。

   > [!NOTE]
   > 请确保“转换 XML”的内容是 XML。 如果内容不是 XML 格式或采用 base64 编码，则必须指定用于处理该内容的表达式。 例如，可以使用[函数](logic-apps-workflow-definition-language.md#functions)（如 ```@base64ToBinary```）来解码内容，或使用 ```@xml``` 来处理 XML 格式的内容。
 

7. 选择要用于执行转换的“映射”的名称。 该映射必须已处于集成帐户中。 在前面的步骤中，已向逻辑应用授予了对包含映射的集成帐户的访问权限。      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. 保存工作  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

此时，已完成映射设置。 在实际应用程序中，可能要将转换的数据存储在 LOB 应用程序中，如 SalesForce。 可以轻松地添加操作以将转换的输出发送给 Salesforce。 

现在可以通过向 HTTP 终结点发出请求来测试转换。  

## <a name="features-and-use-cases"></a>功能和用例
* 在映射中创建的转换可以非常简单，如将名称和地址从一个文档复制到另一个文档。 或者，可以使用现成可用的映射操作创建更复杂的转换。  
* 有多个映射操作或函数现成可用，包括字符串、日期时间函数等等。  
* 可以在架构之间执行直接数据复制。 在 SDK 中包含的映射程序中，这简单到只需绘制一条线将源架构中的元素与目标架构中的对应项连接。  
* 创建映射时，可查看映射的图形表示形式，其中会显示创建的所有关系和链接。
* 使用“测试映射”功能可添加示例 XML 消息。 只需进行简单的单击，便可以测试创建的映射，并查看生成的输出。  
* 上传现有映射  
* 包括对 XML 格式的支持。

## <a name="learn-more"></a>了解详细信息
* [了解有关 Enterprise Integration Pack 的详细信息](../logic-apps/logic-apps-enterprise-integration-overview.md "了解 Enterprise Integration Pack")  
* [了解有关映射的详细信息](../logic-apps/logic-apps-enterprise-integration-maps.md "了解企业集成映射")  

