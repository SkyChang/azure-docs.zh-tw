---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 Datava Enterprise Service Platform 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Datava Enterprise Service Platform 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/20/2020
ms.author: jeedes
ms.openlocfilehash: 7d249005e77328460a0e7152e0a3ff125857534b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89659480"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-datava-enterprise-service-platform"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 Datava Enterprise Service Platform 整合

在本教學課程中，您將了解如何整合 Datava Enterprise Service Platform 與 Azure Active Directory (Azure AD)。 當您將 Datava Enterprise Service Platform 與 Azure AD 整合後，您可以：

* 在 Azure AD 中控制可存取 Datava Enterprise Service Platform 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Datava Enterprise Service Platform。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 Datava Enterprise Service Platform 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Datava Enterprise Service Platform 支援 **SP** 起始的 SSO
* Datava Enterprise Service Platform 支援 **Just In Time** 使用者佈建
* 設定 Datava Enterprise Service Platform 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。

> [!NOTE]
> 此應用程式的識別碼是固定的字串值，因此一個租用戶中只能設定一個執行個體。

## <a name="adding-datava-enterprise-service-platform-from-the-gallery"></a>從資源庫新增 Datava Enterprise Service Platform

若要進行將 Datava Enterprise Service Platform 整合到 Azure AD 中的設定，您必須從資源庫將 Datava Enterprise Service Platform 新增至受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory] 服務。
1. 巡覽至 [企業應用程式]，然後選取 [所有應用程式]。
1. 若要新增應用程式，請選取 [新增應用程式]。
1. 在 [從資源庫新增] 區段的搜尋方塊中，輸入 **Datava Enterprise Service Platform**。
1. 從結果面板中選取 [Datava Enterprise Service Platform]，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-sso-for-datava-enterprise-service-platform"></a>設定及測試適用於 Datava Enterprise Service Platform 的 Azure AD SSO

以名為 **B.Simon** 的測試使用者，設定及測試與 Datava Enterprise Service Platform 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Datava Enterprise Service Platform 中相關使用者之間的連結關聯性。

若要設定及測試與 Datava Enterprise Service Platform 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 Datava Enterprise Service Platform SSO](#configure-datava-enterprise-service-platform-sso)** - 在應用程式端設定單一登入設定。
    1. **[建立 Datava Enterprise Service Platform 測試使用者](#create-datava-enterprise-service-platform-test-user)** - 使 Datava Enterprise Service Platform 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Datava Enterprise Service Platform] 應用程式整合頁面上，尋找 [管理] 區段並選取 [單一登入]。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入] 頁面上，按一下 [基本 SAML 設定] 的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態] 區段上，輸入下列欄位的值：

    a. 在 [登入 URL] 文字方塊中，以下列模式輸入 URL︰`https://go.datava.com/<TENANT_NAME>`

    b. 在 [回覆 URL] 文字方塊中，輸入 URL：`https://go.datava.com/saml/module.php/saml/sp/saml2-acs.php/azure-sp`

    > [!NOTE]
    > 這不是真正的值。 請使用實際的「登入 URL」來更新此值。 請連絡 [Datava Enterprise Service Platform 用戶端支援小組](mailto:support@datava.com)以取得此值。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

1. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中，按一下 [複製] 按鈕以複製 [應用程式同盟中繼資料 URL]，並將資料儲存在您的電腦上。

    ![憑證下載連結](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]、[使用者] 和 [所有使用者]。
1. 在畫面頂端選取 [新增使用者]。
1. 在 [使用者] 屬性中，執行下列步驟：
   1. 在 [名稱] 欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱] 欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼] 核取方塊，然後記下 [密碼] 方塊中顯示的值。
   1. 按一下頁面底部的 [新增] 。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Datava Enterprise Service Platform 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]，然後選取 [所有應用程式]。
1. 在應用程式清單中，選取 [Datava Enterprise Service Platform]。
1. 在應用程式的概觀頁面中尋找 [管理] 區段，然後選取 [使用者和群組]。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]，然後在 [新增指派] 對話方塊中選取 [使用者和群組]。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組] 對話方塊的 [使用者] 清單中選取 [B.Simon]，然後按一下畫面底部的 [選取] 按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色] 對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取] 按鈕。
1. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

## <a name="configure-datava-enterprise-service-platform-sso"></a>設定 Datava Enterprise Service Platform SSO

若要在 **Datava Enterprise Service Platform** 端設定單一登入，您必須將 [應用程式同盟中繼資料 URL] 傳送給 [Datava Enterprise Service Platform支援小組](mailto:support@datava.com)。 他們會進行此設定，讓兩端的 SAML SSO 連線都設定正確。

### <a name="create-datava-enterprise-service-platform-test-user"></a>建立 Datava Enterprise Service Platform 測試使用者

本節會在 Datava Enterprise Service Platform 中建立名為 Britta Simon 的使用者。 Datava Enterprise Service Platform 支援依預設啟用的 Just-In-Time 使用者佈建。 在這一節沒有您需要進行的動作項目。 如果 Datava Enterprise Service Platform 中還沒有任何使用者存在，在驗證之後就會建立新的使用者。

## <a name="test-sso"></a>測試 SSO

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Datava Enterprise Service Platform] 圖格時，應該會自動登入您已設定 SSO 的 Datava Enterprise Service Platform。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [嘗試搭配 Azure AD 使用 Datava Enterprise Service Platform](https://aad.portal.azure.com/)

- [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
