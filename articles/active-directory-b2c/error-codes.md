---
title: 錯誤碼參考
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C 服務可傳回的錯誤碼清單。
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 10/02/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 0122fa43c9d99c01797e3523748e4f31b4b7469a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91664846"
---
# <a name="error-codes-azure-active-directory-b2c"></a>錯誤碼： Azure Active Directory B2C

Azure Active Directory B2C 服務可能會傳回下列錯誤。

| 錯誤碼 | 訊息 |
| ---------- | ------- |
| `AADB2C90002` | CORS 資源 ' {0} ' 傳回404找不到。 |
| `AADB2C90006` | 要求中提供的重新導向 URI ' {0} ' 未註冊用戶端識別碼 ' {1} '。 |
| `AADB2C90007` | 與用戶端識別碼 ' ' 相關聯的應用程式 {0} 沒有註冊的重新導向 uri。 |
| `AADB2C90008` | 要求不包含用戶端識別碼參數。 |
| `AADB2C90010` | 要求未包含範圍參數。 |
| `AADB2C90011` | 要求中提供的用戶端識別碼 ' ' 不 {0} 符合 {1} 原則中註冊的用戶端識別碼 ' '。 |
| `AADB2C90012` | {0}要求中提供的範圍 ' ' 不受支援。 |
| `AADB2C90013` | 要求中提供的要求回應類型 ' {0} ' 不受支援。 |
| `AADB2C90014` | 要求中提供的要求回應模式 ' {0} ' 不受支援。 |
| `AADB2C90016` | 要求的用戶端判斷提示類型 ' {0} ' 不符合預期的類型 ' ' {1} 。 |
| `AADB2C90017` | 要求中提供的用戶端判斷提示無效： {0} |
| `AADB2C90018` | 要求中指定的用戶端識別碼 ' {0} ' 未在租使用者 ' ' 中註冊 {1} 。 |
| `AADB2C90019` | 租使用者 ' ' 中識別碼為 ' ' 的金鑰容器沒有 {0} {1} 有效的金鑰。 原因： {2} 。 |
| `AADB2C90021` | 技術設定檔 ' {0} ' 不存在於租使用者 ' ' 的原則 ' ' 中 {1} {2} 。 |
| `AADB2C90022` | 無法傳回 {0} 租使用者 ' ' 中原則 ' ' 的中繼資料 {1} 。 |
| `AADB2C90023` | 設定檔 ' {0} ' 未包含必要的中繼資料索引鍵 ' {1} '。 |
| `AADB2C90025` | 租使用者 {0} ' ' 中原則 ' ' 中的設定檔 ' ' {1} {2} 未包含必要的密碼編譯金鑰 ' {3} '。 |
| `AADB2C90027` | 為 ' ' 指定的基本認證 {0} 無效。 請檢查認證是否正確，以及資源是否已授與存取權。 |
| `AADB2C90028` | 為 ' ' 指定的用戶端憑證 {0} 無效。 檢查憑證是否正確、是否包含私密金鑰，以及資源是否已授與存取權。 |
| `AADB2C90031` | 原則 ' {0} ' 未指定預設使用者旅程圖。 確定原則或其父代將預設使用者旅程圖指定為信賴憑證者區段的一部分。 |
| `AADB2C90035` | 服務暫時無法使用。 請于幾分鐘後重試。 |
| `AADB2C90036` | 要求未包含可將使用者重新導向至 post 登出的 URI。 在 [post_logout_redirect_uri 參數] 欄位中指定 URI。 |
| `AADB2C90037` | 處理這個要求時發生錯誤。 請聯絡您嘗試存取的網站系統管理員。 |
| `AADB2C90039` | 要求包含用戶端判斷提示，但在租使用者 ' ' 中提供的原則 ' ' {0} {1} 缺少 RelyingPartyPolicy 中的 client_secret。 |
| `AADB2C90040` | 使用者旅程圖 ' {0} ' 不包含傳送宣告步驟。 |
| `AADB2C90043` | 要求中包含的提示包含不正確值。 應為 ' none '、' login ' '、' 同意 ' 或 ' select_account '。 |
| `AADB2C90044` | 宣告 {0} 解析程式 ' ' 不支援宣告 ' ' {1} 。 |
| `AADB2C90046` | 載入您目前的狀態時發生問題。 您可能想要從頭開始嘗試啟動您的會話。 |
| `AADB2C90047` | 資源 ' {0} ' 包含防止載入的腳本錯誤。 |
| `AADB2C90048` | 伺服器上發生未處理的例外狀況。 |
| `AADB2C90051` | 找不到適合的宣告提供者。 |
| `AADB2C90052` | 無效的使用者名稱或密碼。 |
| `AADB2C90053` | 找不到具有指定認證的使用者。 |
| `AADB2C90054` | 無效的使用者名稱或密碼。 |
| `AADB2C90055` | {0}要求中提供的範圍 ' ' 必須指定資源，例如 ' https://example.com/calendar.read '。 |
| `AADB2C90057` | 提供的應用程式未設定為允許 OAuth 隱含流程。 |
| `AADB2C90058` | 提供的應用程式未設定為允許公開用戶端。 |
| `AADB2C90067` | Post 登出重新導向 URI ' {0} ' 的格式無效。 指定以 HTTPs 為基礎的 URL （例如 ' '）， https://example.com/return 或針對原生用戶端使用 IETF native CLIENT URI ' urn： ietf： wg： oauth：2.0： oob '。 |
| `AADB2C90068` | 提供的應用程式識別碼 ' {0} ' 對此服務無效。 請使用透過 B2C 入口網站建立的應用程式，然後再試一次。 |
| `AADB2C90075` | {0}在步驟 ' ' 中指定的宣告交換 ' ' {1} 傳回 HTTP 錯誤回應，代碼為 ' {2} '，原因為 ' {3} '。 |
| `AADB2C90077` | 使用者沒有現有的會話，且要求提示參數的值為 ' {0} '。 |
| `AADB2C90079` | 用戶端必須在兌換機密授與時傳送 client_secret。 |
| `AADB2C90080` | 提供的授與已過期。 請重新驗證並再試一次。 目前時間： {0} ，授與發出的時間： {1} ，授與滑動視窗到期時間： {2} 。 |
| `AADB2C90081` | 指定的 client_secret 與此用戶端預期的值不符。 請更正 client_secret，然後再試一次。 |
| `AADB2C90083` | 要求遺漏必要參數： {0} 。 |
| `AADB2C90084` | 公開用戶端在兌換公開取得的授與時，不應傳送 client_secret。 |
| `AADB2C90085` | 服務發生內部錯誤。 請重新驗證並再試一次。 |
| `AADB2C90086` | 不支援提供的 grant_type [ {0} ]。 |
| `AADB2C90087` | 未對此版本的通訊協定端點發出提供的授與。 |
| `AADB2C90088` | 未對此端點發出提供的授與。 實際值： {0} 和預期值： {1} |
| `AADB2C90091` | 使用者取消。 |
| `AADB2C90092` | 為 {0} 租使用者 ' ' 停用了識別碼為 ' ' 的提供的應用程式 {1} 。 請啟用應用程式，然後再試一次。 |
| `AADB2C90107` | 識別碼為 ' ' 的應用程式 {0} 無法取得識別碼權杖，原因是要求中未提供 openid 範圍，或未授權該應用程式。 |
| `AADB2C90108` | 協調流程步驟 ' {0} ' 未指定 CpimIssuerTechnicalProfileReferenceId 時，必須指定一個。 |
| `AADB2C90110` | 要求包含 ' id_token ' 的 response_type 時，範圍參數必須包含 ' openid '。 |
| `AADB2C90111` | 您的帳戶已遭鎖定。 請連絡您的支援人員將其解除鎖定，然後再試一次。 |
| `AADB2C90114` | 您的帳戶已暫時鎖定以防未經授權的使用。 請稍後再試。 |
| `AADB2C90115` | 要求 ' code ' response_type 時，範圍參數必須包含存取權杖的資源或用戶端識別碼，以及識別碼權杖的 ' openid '。 此外，還包括重新整理權杖的 ' offline_access '。 |
| `AADB2C90117` | {0}要求中提供的範圍 ' ' 不受支援。 |
| `AADB2C90118` | 使用者忘記其密碼。 |
| `AADB2C90120` | 要求中指定的最大壽命參數 ' {0} ' 無效。 最大存留期必須是介於 ' {1} ' 和 ' {2} ' （含）之間的整數。 |
| `AADB2C90122` | 在 {0} 要求中收到的 ' ' 輸入具有失敗的 HTTP 要求驗證。 請確定輸入不包含字元，例如 < 或 &。 |
| `AADB2C90128` | 與此授與相關聯的帳戶已不再存在。 請重新驗證並再試一次。 |
| `AADB2C90129` | 提供的授與已撤銷。 請重新驗證並再試一次。 |
| `AADB2C90145` | 找不到未驗證的電話號碼，而且原則不允許使用者輸入的號碼。 |
| `AADB2C90146` | {0}要求中提供的範圍 ' ' 針對存取權杖指定了多個資源，但不支援此項。 |
| `AADB2C90149` | {0}無法載入腳本 ' '。 |
| `AADB2C90151` | 使用者已超過多重要素驗證重試次數的上限。 |
| `AADB2C90152` | 多重要素輪詢要求無法從服務取得回應。 |
| `AADB2C90154` | 多重要素驗證要求無法從服務取得會話識別碼。 |
| `AADB2C90155` | 多重要素驗證要求失敗，原因為 ' {0} '。 |
| `AADB2C90156` | 多重要素驗證要求失敗，原因為 ' {0} '。 |
| `AADB2C90157` | 使用者已超過自行判斷提示步驟的重試次數上限。 |
| `AADB2C90158` | 自我判斷的驗證要求失敗，原因為 ' {0} '。 |
| `AADB2C90159` | 自我判斷驗證要求失敗，原因為 ' {0} '。 |
| `AADB2C90161` | 自我判斷的傳送回應失敗，原因為 ' {0} '。 |
| `AADB2C90165` | 在狀態中找不到識別碼為 ' ' 的 SAML 起始訊息 {0} 。 |
| `AADB2C90168` | HTTP-Redirect 要求未包含已簽署要求的必要參數 ' {0} '。 |
| `AADB2C90178` | 簽署憑證 ' {0} ' 沒有私用金鑰。 |
| `AADB2C90182` | 提供的 code_verifier 不符合相關聯的 code_challenge |
| `AADB2C90183` | 提供的 code_verifier 無效 |
| `AADB2C90184` | 不支援提供的 code_challenge_method。 支援的值為純文字或 S256 |
| `AADB2C90188` | SAML 技術設定檔 ' {0} ' 指定了 ' ' 的 PARTNERENTITYURL URL {1} ，但提取中繼資料失敗，原因為 ' {2} '。 |
| `AADB2C90194` | 為 {0} 持有人權杖指定的宣告 ' ' 不存在於可用的宣告中。 可用的宣告 ' {1} '。 |
| `AADB2C90205` | 此應用程式沒有足夠的許可權可對此 web 資源執行此作業。 |
| `AADB2C90206` | 初始化用戶端時所發生的超時時間。 |
| `AADB2C90208` | 提供的 id_token_hint 參數已過期。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90209` | 提供的 id_token_hint 參數未包含已接受的物件。 有效的物件值： ' {0} '。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90210` | 無法驗證提供的 id_token_hint 參數。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90211` | 要求包含未完成的狀態 cookie。 |
| `AADB2C90212` | 要求包含不正確狀態 cookie。 |
| `AADB2C90220` | 租使用者 ' {0} ' 中儲存體識別碼為 ' ' 的金鑰容器 {1} 存在，但不包含有效的憑證。 憑證可能已過期，或您的憑證可能會在未來的 (nbf) 生效。 |
| `AADB2C90223` | 淨化 CORS 資源時發生錯誤。 |
| `AADB2C90224` | 尚未為應用程式啟用資源擁有者流程。 |
| `AADB2C90225` | 要求中提供的使用者名稱或密碼無效。 |
| `AADB2C90226` | 只有透過 HTTP POST 才支援指定的權杖交換。 |
| `AADB2C90232` | 提供的 id_token_hint 參數未包含已接受的簽發者。 有效的簽發者： ' {0} '。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90233` | 提供的 id_token_hint 參數簽章驗證失敗。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90235` | 提供的 id_token 已過期。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90237` | 提供的 id_token 不包含有效的物件。 有效的物件值： ' {0} '。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90238` | 提供的 id_token 不包含有效的簽發者。 有效的簽發者值： ' {0} '。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90239` | 提供的 id_token 簽章驗證失敗。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90240` | 提供的 id_token 格式不正確，無法剖析。 請提供另一個權杖，然後再試一次。 |
| `AADB2C90242` | SAML 技術設定檔 ' {0} ' 指定無法載入的 PARTNERENTITYURL CDATA，原因為 ' {1} '。 |
| `AADB2C90243` | IDP 的用戶端金鑰/密碼未正確設定。 |
| `AADB2C90244` | 目前的要求過多。 請稍後再重試。 |
| `AADB2C90248` | 只有透過 B2C 系統管理員入口網站建立的應用程式，才能使用資源擁有者流程。 |
| `AADB2C90250` | 不支援一般登入端點。 |
| `AADB2C90255` | 技術設定檔 ' ' 中指定的宣告交換 {0} 未如預期般完成。 您可能想要從頭開始嘗試啟動您的會話。 |
| `AADB2C90261` | {0}在步驟 ' ' 中指定的宣告交換 ' ' {1} 傳回了無法剖析的 HTTP 錯誤回應。 |
| `AADB2C90272` | 未在要求中指定 id_token_hint 參數。 請提供權杖，然後再試一次。 |
| `AADB2C90273` | 收到不正確回應： ' {0} ' |
| `AADB2C90274` | 提供者中繼資料未指定單一登出服務，或端點系結不是 ' urn： oasis： names： tc： SAML：2.0： binding： HTTP-重新導向 ' 或 ' urn： oasis： names： tc： SAML：2.0： binding： HTTP-POST ' 中的其中一個。 |
| `AADB2C90276` | 要求與 {0} {1} {2} 原則 ' {3} ' tenant ' ' 的 technicalProfile ' ' 中的控制設定 ' '： ' ' 不一致 {4} 。 |
| `AADB2C90277` | 原則 ' ' 的協調流程步驟「使用者旅程」 ' 不 {0} {1} {2} 包含內容定義參考。 |
| `AADB2C90279` | 提供的用戶端識別碼 ' {0} ' 不符合發出授與的用戶端識別碼。 |
| `AADB2C90284` | 識別碼為 ' ' 的應用程式 {0} 未獲得同意，且無法用於本機帳戶。 |
| `AADB2C90285` | 找不到識別碼為 ' ' 的應用程式 {0} 。 |
| `AADB2C90288` | {0}在 TechnicalProfile ' ' 中參考的識別碼為 ' ' 的 UserJourney 中 {1} ，租使用者 ' ' 的重新整理權杖兌換 {2} 不存在於原則 ' {3} ' 或其任何基礎原則中。 |
| `AADB2C90289` | 連接到身分識別提供者時發生錯誤。 請稍後再試一次。 |
| `AADB2C90296` | 尚未正確設定應用程式。 請聯絡您嘗試存取的網站系統管理員。 |
| `AADB2C99005` | 要求包含不正確範圍參數，其中包含不合法的字元 ' {0} '。 |
| `AADB2C99006` | Azure AD B2C 找不到應用程式識別碼為 ' ' 的擴充功能應用程式 {0} 。 如需 https://go.microsoft.com/fwlink/?linkid=851224 詳細資訊，請造訪。 |
| `AADB2C99011` | {0}未在 {1} 原則 ' ' 中的 TechnicalProfile ' ' 中指定中繼資料值 ' ' {2} 。 |
| `AADB2C99013` | 不支援提供的 grant_type [ {0} ] 和 token_type [ {1} ] 組合。 |
| `AADB2C99015` | 租使用者 ' ' 中原則 ' ' 的設定檔 ' ' {0} {1} {2} 缺少資源擁有者密碼認證流程所需的所有 InputClaims。 |
