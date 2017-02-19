---
title: "Azure Active Directory B2B 协作故障排除 | Microsoft 文档"
description: "Azure Active Directory B2B 协作的常见问题的补救措施"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/02/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: d48034fdefdfdb93d99d5524e3272fc53ce5b49f
ms.openlocfilehash: 03ca49b31efe3a003b2f30dbfe27d65f90379d5c


---

# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B 协作故障排除

以下是 Azure Active Directory (Azure AD) B2B 协作的常见问题的一些补救措施。

## <a name="i-cant-create-an-external-user-due-to-an-existing-contact"></a>由于存在现有联系人，无法创建外部用户

如果你要邀请的外部用户已有预先存在的联系人对象，你将无法邀请该用户，直到你解决冲突（通常是通过删除该联系人对象来解决）。 在正式发布 B2B 协作之前，必须手动解决该冲突。

## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>我已添加外部用户，但在全局通讯簿或人员选取器中看不到这些用户

在外部用户未填充到列表中的情况下，可能需要几分钟复制对象。

## <a name="invitations-have-been-disabled-for-directory"></a>已对目录禁用邀请

如果你收到错误消息指示你无权邀请用户，请验证你的用户帐户是否有权邀请外部用户。 这可以在“用户设置”下完成：

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

如果你最近修改了这些设置或为用户分配了“来宾邀请者”角色，可能有 15-60 分钟的延迟更改才生效。

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>我邀请的用户在兑换过程中收到错误

常见错误包括：

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>当发生以下情况时，被邀请者的管理员禁止在其租户中创建电子邮件验证的用户：

受邀用户所在组织正在利用不存在特定用户帐户的 Azure Active Directory（用户不存在于 AAD contoso.com 中）。 contoso.com 的管理员可能会设置一个策略以阻止创建用户。 外部用户必须咨询其管理员以确定是否允许外部用户，外部用户的管理员可能需要允许在其域中创建电子邮件验证的用户（有关允许电子邮件验证的用户，请参阅此[文章](https://docs.microsoft.com/en-us/powershell/msonline/v1/set-msolcompanysettings#parameters)）。

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>外部用户尚未存在于联合域中

在外部用户正在使用联合身份验证解决方案的情况下，在该解决方案中，身份验证在本地执行，而该用户尚未存在于 Azure Active Directory 中，因此无法邀请该用户。

若要解决此问题，外部用户的管理员必须将该用户的帐户同步到 Azure Active Directory。

## <a name="how-does--which-is-normally-an-invalid-character-sync-with-azure-ad"></a>“\#”（这通常是一个无效字符）如何与 Azure AD 同步？

“\#”是 Azure AD B2B 协作或外部用户的 UPN 中的保留字符（即，如果 &lt;user@contoso.com&gt; 邀请，将变为 &lt;user_contoso.com#EXT@fabrikam.onmicrosoft.com&gt;），因此不允许来自本地的 UPN 中的 \# 登录到 Azure 门户。

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>我将外部用户添加到同步组时，收到错误

外部用户只能添加到“已分配”或“安全”组，而不能分配到在本地控制的组。

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>我的外部用户未收到用于兑换的电子邮件

被邀请者应该向其 ISP 或垃圾邮件筛选器查询，以确保允许以下地址：&lt;Invites@microsoft.com&gt;

## <a name="my-recipient-received-multiple-emails-from-me"></a>我的收件人收到了我的多个电子邮件

在某些情况下，邀请收件人的帐户有多个别名，他们可能会收到两份邀请。 在这些情况下，已兑换的第一个链接是获得创建的帐户，第二个兑换链接那时将无效。

## <a name="next-steps"></a>后续步骤

在 Azure AD B2B 协作网站上浏览我们的其他文章：

* [什么是 Azure AD B2B 协作？](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure Active Directory 管理员如何添加 B2B 协作用户？](active-directory-b2b-admin-add-users.md)
* [信息工作者如何添加 B2B 协作用户？](active-directory-b2b-how-it-works.md)
* [B2B 协作邀请电子邮件的元素](active-directory-b2b-invitation-email.md)
* [B2B 协作邀请兑换](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B 协作授权](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B 协作常见问题 (FAQ)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B 协作 API 和自定义](active-directory-b2b-api.md)
* [适用于 B2B 协作用户的多重身份验证](active-directory-b2b-mfa-instructions.md)
* [有关 Azure Active Directory 中应用程序管理的文章索引](active-directory-apps-index.md)



<!--HONumber=Feb17_HO1-->

