---
title: 將 Azure AD 角色指派給使用者-Azure Active Directory |Microsoft Docs
description: 有關如何使用 Azure Active Directory 將系統管理員和非系統管理員角色指派給使用者的指示。
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 08/31/2020
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: fb7ab83bc9939d2f0b4b0ff0860ea97a0b07f12f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89321237"
---
# <a name="assign-administrator-and-non-administrator-roles-to-users-with-azure-active-directory"></a>使用 Azure Active Directory 將系統管理員和非系統管理員角色指派給使用者

在 Azure Active Directory (Azure AD) 中，如果您的其中一位使用者需要管理 Azure AD 資源的許可權，您必須將其指派給提供所需許可權的角色。 如需管理 Azure 資源的角色，以及哪些角色管理 Azure AD 資源的相關資訊，請參閱 [傳統訂用帳戶管理員角色、Azure 角色和 Azure AD 角色](../../role-based-access-control/rbac-and-directory-admin-roles.md)。

如需可用 Azure AD 角色的詳細資訊，請參閱 [Azure Active Directory 中指派系統管理員角色](../users-groups-roles/directory-assign-admin-roles.md)。 若要新增使用者，請參閱 [將新使用者新增至 Azure Active Directory](add-users-azure-active-directory.md)。

## <a name="assign-roles"></a>指派角色

將 Azure AD 角色指派給使用者的常見方式是在使用者的 [ **指派的角色** ] 頁面上。 您也可以使用 Privileged Identity Management (PIM) ，將使用者資格設定為及時提升為角色。 如需有關如何使用 PIM 的詳細資訊，請參閱 [Privileged Identity Management](../privileged-identity-management/index.yml)。

> [!Note]
> 如果您有 Azure AD Premium P2 授權方案，並已使用 PIM，則會在 [Privileged Identity Management 體驗](../users-groups-roles/directory-manage-roles-portal.md)中執行所有角色管理工作。 這項功能目前僅限一次只能指派一個角色。 您目前無法選取多個角色，並一次將它們指派給使用者。
>
> ![在 PIM 中針對已使用 PIM 並擁有 Premium P2 授權的使用者，Azure AD 管理的角色](./media/active-directory-users-assign-role-azure-portal/pim-manages-roles-for-p2.png)

## <a name="assign-a-role-to-a-user"></a>將角色指派給使用者

1. 移至 [Azure 入口網站](https://portal.azure.com/) ，並使用目錄的全域系統管理員帳戶登入。

2. 搜尋並選取 [Azure Active Directory]。

      ![Azure 入口網站搜尋 Azure Active Directory](media/active-directory-users-assign-role-azure-portal/search-azure-active-directory.png)

3. 選取 [使用者]。

4. 搜尋並選取取得角色指派的使用者。 例如 _Alain Charon_。

      ![所有使用者頁面-選取使用者](media/active-directory-users-assign-role-azure-portal/directory-role-select-user.png)

5. 在 [ **Alain Charon-設定檔** ] 頁面上，選取 [ **指派的角色**]。

    [ **Alain Charon-系統管理角色** ] 頁面隨即出現。

6. 選取 [ **新增指派**]，選取要指派給 Alain (的角色，例如 _應用程式系統管理員_) ]，然後選擇 [ **選取**]。

    ![指派的角色頁面-顯示選取的角色](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    系統會將應用程式系統管理員角色指派給 Alain Charon，並顯示在 [ **Alain Charon-系統管理角色** ] 頁面上。

## <a name="remove-a-role-assignment"></a>移除角色指派

如果您需要從使用者移除角色指派，也可以從 [ **Alain Charon-系統管理角色** ] 頁面進行。

### <a name="to-remove-a-role-assignment-from-a-user"></a>若要移除使用者的角色指派

1. 選取 [Azure Active Directory]**** 並選取 [使用者]****，然後搜尋並選取要移除角色指派的使用者。 例如 _Alain Charon_。

2. 選取 [ **指派的角色**]，選取 [ **應用程式系統管理員**]，然後選取 [ **移除指派**]。

    ![指派的角色頁面，顯示選取的角色和移除選項](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    應用程式系統管理員的角色會從 Alain Charon 中移除，且不會再出現在 [ **Alain Charon-系統管理角色** ] 頁面上。

## <a name="next-steps"></a>後續步驟

- [新增或刪除使用者](add-users-azure-active-directory.md)

- [新增或變更設定檔資訊](active-directory-users-profile-azure-portal.md)

- [從另一個目錄中新增來賓使用者](../external-identities/what-is-b2b.md)

您可以查看的其他使用者管理工作可在 [Azure Active Directory 使用者管理檔](../users-groups-roles/index.yml)中取得。