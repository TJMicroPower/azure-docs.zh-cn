---
title: "教程：Azure Active Directory 与 Cerner Central 的集成 | Microsoft Docs"
description: "了解如何在 Azure Active Directory 和 Cerner Central 之间配置单一登录。"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2bc549d-d286-4679-854e-bb67c62b0475
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: f01e605d2d3f68f7e95839dab78b3b10e43d28f4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cerner-central"></a>教程：Azure Active Directory 与 Cerner Central 的集成

本教程介绍了如何将 Cerner Central 与 Azure Active Directory (Azure AD) 进行集成。

将 Cerner Central 与 Azure AD 集成可提供以下优势：

- 可以在 Azure AD 中控制谁有权访问 Cerner Central
- 可以让用户使用其 Azure AD 帐户自动登录到 Cerner Central（单一登录）
- 可以在一个中心位置（即 Azure 门户）中管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Cerner Central 的集成，需要具有以下项：

- 一个 Azure AD 订阅
- 已批准的 Cerner Central 系统帐户

> [!NOTE]
> 不建议使用生产环境测试本教程中的步骤。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Cerner Central
2. 配置和测试 Azure AD 单一登录

## <a name="adding-cerner-central-from-the-gallery"></a>从库中添加 Cerner Central
要配置 Cerner Central 与 Azure AD 的集成，需要从库中将 Cerner Central 添加到托管 SaaS 应用列表。

**若要从库中添加 Cerner Central，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)**的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
3. 若要添加新的应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

4. 在搜索框中，键入“Cerner Central”。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_search.png)

5. 在结果窗格中，选择“Cerner Central”，并单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置并测试 Cerner Central 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Cerner Central 用户。 换句话说，需要在 Azure AD 用户与 Cerner Central 中相关用户之间建立链接关系。

若要配置和测试 Cerner Central 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Cerner Central 测试用户](#creating-a-cerner-central-test-user)** - 在 Cerner Central 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，会在 Azure 门户中启用 Azure AD 单一登录并在 Cerner Central 应用程序中配置单一登录。

**若要配置 Cerner Central 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中，在 **Cerner Central** 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_samlbase.png)

3. 在“Cerner Central 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_url.png)

    a. 在“标识符”文本框中，使用以下模式键入值：
    
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    

    b. 在“回复 URL”文本框中，使用以下模式键入 URL： 
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/sso` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/sso` |
    

    > [!NOTE] 
    > 这些不是实际值。 使用实际标识符和回复 URL 更新这些值。 请联系 [Cerner Central 支持团队](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations)来获取这些值。
 
4. 单击“保存”按钮。

    ![配置单一登录](./media/active-directory-saas-cernercentral-tutorial/tutorial_general_400.png)

5. 若要生成**元数据** URL，请执行以下步骤：

    a. 单击“应用注册”。
    
    ![配置单一登录](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appregistrations.png)
   
    b. 单击“终结点”以打开“终结点”对话框。  
    
    ![配置单一登录](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpointicon.png)

    c. 单击复制按钮以复制**联合元数据文档** URL 并将其粘贴到记事本。
    
    ![配置单一登录](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpoint.png)
     
    d. 现在，转到 **Cerner Central** 的属性页，使用“复制”按钮复制**应用程序 Id** 并将其粘贴到记事本。
 
    ![配置单一登录](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appid.png)

    e. 使用以下模式生成**元数据 URL**：`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

6. 要在 **Cerner Central** 端配置单一登录，需要将**元数据 URL** 发送给 [Cerner Central 支持团队](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations)。 他们会在应用程序端配置 SSO 以完成集成。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[ Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。 

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_01.png) 

2. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_02.png) 

3. 若要打开“用户”对话框，请单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_03.png) 

4. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 Britta Simon 的**电子邮件地址**。

    c. 选择“显示密码”并记下“密码”的值。

    d.单击“下一步”。 单击“创建” 。
 
### <a name="creating-a-cerner-central-test-user"></a>创建 Cerner Central 测试用户

Cerner Central 应用程序允许从任何联合标识提供者进行身份验证。 如果用户能够登录到应用程序主页，他们将成为联合用户，无需进行任何手动设置。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将通过向 Britta Simon 授予对 Cerner Centra 的访问权限使其能够使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 Cerner Central，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Cerner Central”。

    ![配置单一登录](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_app.png) 

3. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

当在访问面板中单击 Cerner Central 磁贴时，应当会自动登录到 Cerner Central 应用程序。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](active-directory-saas-tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_203.png

