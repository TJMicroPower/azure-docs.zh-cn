---
title: "步骤 1：创建机器学习工作区 | Microsoft Docs"
description: "开发预测解决方案演练的步骤 1：了解如何设置新的 Azure 机器学习工作室工作区。"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: ff857e83a37b95bceb751539bb34e9fb7f202931
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a>演练步骤 1：创建机器学习工作区
这是演练[在 Azure 机器学习中开发预测分析解决方案](walkthrough-develop-predictive-solution.md)的第一步。

1. **创建机器学习工作区**
2. [上传现有数据](walkthrough-2-upload-data.md)
3. [创建新试验](walkthrough-3-create-new-experiment.md)
4. [定型和评估模型](walkthrough-4-train-and-evaluate-models.md)
5. [部署 Web 服务](walkthrough-5-publish-web-service.md)
6. [访问 Web 服务](walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

若要使用机器学习工作室，需要具有 Microsoft Azure 机器学习工作区。 此工作空间包含创建、管理和发布试验所需的工具。  

<!--
## To create a workspace
1. Sign in to the [Azure classic portal](https://manage.windowsazure.com).
2. In the  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On the **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

Azure 订阅的管理员需创建工作区，然后你将添加为所有者或参与者。 有关详细信息，请参阅[创建和共享 Azure 机器学习工作区](create-workspace.md)。

创建工作区后，打开机器学习工作室 ([https://studio.azureml.net/Home](https://studio.azureml.net/Home))。 如果有多个工作区，可在窗口右上角的工具栏中选择工作区。

![在 Studio 中选择工作区][2]

> [!TIP]
> 如果是工作区的所有者，则可通过邀请其他人到工作区，来共享所进行的实验。 可以在“设置”页上的“机器学习工作室”中执行此操作。 仅需要每位用户的 Microsoft 帐户或组织帐户。
> 
> 在“设置”页上，单击“用户”，并在窗口底部单击“邀请更多用户”。
> 
> 

- - -
**下一步：[上传现有数据](walkthrough-2-upload-data.md)**

[1]: ./media/walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/walkthrough-1-create-ml-workspace/open-workspace.png
