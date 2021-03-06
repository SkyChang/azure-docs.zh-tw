---
title: 終端使用者驗證-REST 與 Data Lake Storage Gen1-Azure
description: 了解如何使用 REST API，搭配使用 Azure Active Directory 與 Azure Data Lake Storage Gen1 以完成終端使用者驗證
author: twooley
ms.service: data-lake-store
ms.topic: how-to
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 84e85e6e817972b8ec0bee0e8b441b3585d2d9dd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85984846"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-rest-api"></a>使用 REST API 向 Azure Data Lake Storage Gen1 驗證終端使用者
> [!div class="op_single_selector"]
> * [使用 Java](data-lake-store-end-user-authenticate-java-sdk.md)
> * [使用 .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md)
> * [使用 Python](data-lake-store-end-user-authenticate-python.md)
> * [使用 REST API](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

在本文中，您會了解如何使用 REST API 向 Azure Data Lake Storage Gen1 驗證終端使用者。 如需使用 REST API 向 Data Lake Storage Gen1 進行服務對服務驗證，請參閱[使用 REST API 向 Data Lake Storage Gen1 進行服務對服務驗證](data-lake-store-service-to-service-authenticate-rest-api.md)。

## <a name="prerequisites"></a>Prerequisites

* **Azure 訂用帳戶**。 請參閱[取得 Azure 免費試用](https://azure.microsoft.com/pricing/free-trial/)。

* **建立 Active Directory 「原生」應用程式**。 您必須先完成[使用 Azure Active Directory 向 Data Lake Storage Gen1 進行使用者驗證](data-lake-store-end-user-authenticate-using-active-directory.md)中的步驟。

* **[捲曲](https://curl.haxx.se/)**。 此文章使用 cURL 示範如何對 Data Lake Storage Gen1 進行 REST API 呼叫。

## <a name="end-user-authentication"></a>終端使用者驗證
如果您需要讓使用者利用 Azure AD 登入您的應用程式，建議使用終端使用者驗證。 您的應用程式能夠以與登入的使用者相同的存取層級，來存取 Azure 資源。 使用者必須定期提供其認證，您的應用程式才能繼續存取。

讓終端使用者登入的結果，是系統會提供一個存取權杖和一個重新整理權杖給您的應用程式。 存取權杖會附加到每個對 Data Lake Storage Gen1 或 Data Lake Analytics 提出的要求，預設的有效期是一小時。 重新整理權杖可用來取得新的存取權杖，預設的有效期是二小時，如果定期使用則最多兩週。 您可以使用兩種不同方法讓終端使用者登入。

在此案例中，應用程式會提示使用者登入，且會在使用者的內容中執行所有作業。 請執行下列步驟：

1. 透過您的應用程式，將使用者重新導向至下列 URL：

    `https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>`

   > [!NOTE]
   > \<REDIRECT-URI> 需要經過編碼才能在 URL 中使用。 所以，針對 https://localhost 使用 `https%3A%2F%2Flocalhost`)

    本教學課程的目的是讓您取代以上 URL 中的預留位置值，並將此值貼在網路瀏覽器網址列中。 系統會將您重新導向，以使用 Azure 登入資料來進行驗證。 一旦成功登入，回應會顯示在瀏覽器網址列中。 回應格式如下：

    `http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>`

2. 擷取回應中的授權碼。 在本教學課程中，您可以從網路瀏覽器的網址列中複製授權碼，並在 POST 要求中傳遞至權杖端點，如下列程式碼片段所示：

    ```console
    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
    -F redirect_uri=<REDIRECT-URI> \
    -F grant_type=authorization_code \
    -F resource=https://management.core.windows.net/ \
    -F client_id=<APPLICATION-ID> \
    -F code=<AUTHORIZATION-CODE>
    ```

   > [!NOTE]
   > 在此情況下， \<REDIRECT-URI> 不需要編碼。
   > 
   > 

3. 回應為 JSON 物件，包含存取權杖 (例如 `"access_token": "<ACCESS_TOKEN>"`) 重新整理權杖 (例如：`"refresh_token": "<REFRESH_TOKEN>"`)。 您的應用程式會在存取 Azure Data Lake Storage Gen1 時使用存取權杖，並會在存取權杖過期時重新整理以取得另一個存取權杖。

    ```json
    {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
    ```

4. 存取權杖過期時，您可以使用重新整理權杖要求新的存取權杖，如下列程式碼片段所示：

    ```console
    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
         -F grant_type=refresh_token \
         -F resource=https://management.core.windows.net/ \
         -F client_id=<APPLICATION-ID> \
         -F refresh_token=<REFRESH-TOKEN>
    ```

如需互動使用者驗證的詳細資料，請參閱 [授權碼授與流程](https://msdn.microsoft.com/library/azure/dn645542.aspx)。

## <a name="next-steps"></a>接下來的步驟
在本文中，您已了解如何使用 REST API，使用服務對服務驗證向 Azure Data Lake Storage Gen1 驗證。 您現在可以查看下列文章，了解如何搭配使用 REST API 與 Azure Data Lake Storage Gen1。

* [使用 REST API 在 Data Lake Storage Gen1 上進行帳戶管理作業](data-lake-store-get-started-rest-api.md)
* [使用 REST API 在 Data Lake Storage Gen1 上進行資料作業](data-lake-store-data-operations-rest-api.md)

