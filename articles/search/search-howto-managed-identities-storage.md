---
title: 使用受控識別設定儲存體帳戶的連接
titleSuffix: Azure Cognitive Search
description: 瞭解如何使用受控識別來設定 Azure 儲存體帳戶的索引子連接
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 770ded494f050631cd1c373f4b3fa5846c65e01a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91275127"
---
# <a name="set-up-a-connection-to-an-azure-storage-account-using-a-managed-identity"></a>使用受控識別來設定與 Azure 儲存體帳戶的連線

此頁面描述如何使用受控識別來設定 Azure 儲存體帳戶的索引子連線，而不是在資料來源物件連接字串中提供認證。

在深入了解此功能之前，建議您先了解索引子是什麼，以及如何設定資料來源的索引子。 您可以在下列連結找到更多資訊：
* [索引子概觀](search-indexer-overview.md)
* [Azure Blob 索引子](search-howto-indexing-azure-blob-storage.md)
* [Azure Data Lake Storage Gen2 索引子](search-howto-index-azure-data-lake-storage.md)
* [Azure 資料表索引子](search-howto-indexing-azure-tables.md)

## <a name="set-up-the-connection"></a>設定連線

### <a name="1---turn-on-system-assigned-managed-identity"></a>1 - 開啟系統指派的受控識別

當系統指派的受控識別啟用時，Azure 會為您的搜尋服務建立身分識別，以便用來向相同租用戶和訂用帳戶內的其他 Azure 服務進行驗證。 接著，您可以在角色型存取控制 (RBAC) 指派中使用此身分識別，以允許在編制索引期間存取資料。

![開啟系統指派的受控識別](./media/search-managed-identities/turn-on-system-assigned-identity.png "開啟系統指派的受控識別")

選取 [儲存] 之後，您會看到已指派給搜尋服務的物件識別碼。

![物件識別碼](./media/search-managed-identities/system-assigned-identity-object-id.png "物件識別碼")
 
### <a name="2---add-a-role-assignment"></a>2 - 新增角色指派

在此步驟中，您將提供您 Azure 認知搜尋服務的權限，以從您的儲存體帳戶讀取資料。

1. 在 Azure 入口網站中，瀏覽至包含您想要編制索引之資料的儲存體帳戶。
2. 選取 [存取控制 (IAM)]
3. 選取 [新增]，然後選取 [新增角色指派]

    ![新增角色指派](./media/search-managed-identities/add-role-assignment-storage.png "新增角色指派")

4. 根據您想要編制索引的儲存體帳戶類型，選取適當的角色：
    1. Azure Blob 儲存體會要求您將搜尋服務新增至 **儲存體 Blob 資料讀取器** 角色。
    1. Azure Data Lake Storage Gen2 要求您將搜尋服務新增至 **儲存體 Blob 資料讀取器** 角色。
    1. Azure 資料表儲存體會要求您將搜尋服務新增至 **讀取器和資料存取** 角色。
5.  將 [存取權指派對象為] 保留為 [Azure AD 使用者、群組或服務主體]
6.  搜尋您的搜尋服務，加以選取，然後選取 [儲存]

    Azure Blob 儲存體和 Azure Data Lake Storage Gen2 的範例：

    ![新增儲存體 Blob 資料讀者角色指派](./media/search-managed-identities/add-role-assignment-storage-blob-data-reader.png "新增儲存體 Blob 資料讀者角色指派")

    Azure 資料表儲存體的範例：

    ![新增讀者及資料存取角色指派](./media/search-managed-identities/add-role-assignment-reader-and-data-access.png "新增讀者及資料存取角色指派")

### <a name="3---create-the-data-source"></a>3 - 建立資料來源

[REST API](/rest/api/searchservice/create-data-source)、Azure 入口網站和[.net SDK](/dotnet/api/microsoft.azure.search.models.datasource)都支援受控識別連接字串。 以下範例說明如何建立資料來源，以使用 [REST API](/rest/api/searchservice/create-data-source) 和受控識別連接字串，從儲存體帳戶編制資料的索引。 REST API、.NET SDK 和 Azure 入口網站的受控識別連接字串格式都相同。

從儲存體帳戶編製索引時，資料來源必須具有下列必要屬性︰

* **名稱**是搜尋服務中資料來源的唯一名稱。
* **type**
    * Azure Blob 儲存體：`azureblob`
    * Azure 資料表儲存體：`azuretable`
    * Azure Data Lake Storage Gen2：當您使用[此表單](https://aka.ms/azure-cognitive-search/mi-preview-request)註冊預覽版之後，將會提供**類型**。
* **credentials**
    * 使用受控識別進行驗證時，**credentials** 格式會與未使用受控身分識別時的格式不同。 您將這裡提供沒有帳戶金鑰或密碼的 ResourceId。 ResourceId 必須包含儲存體帳戶的訂用帳戶識別碼、儲存體帳戶的資源群組，以及儲存體帳戶名稱。
    * 受控識別格式： 
        * *ResourceId=/subscriptions/**您的訂用帳戶識別碼**/resourceGroups/**您的資源群組名稱**/providers/Microsoft.Storage/storageAccounts/**您的儲存體帳戶名稱**/;*
* **容器**會指定您儲存體帳戶中的容器或資料表名稱。 根據預設，容器內的所有 Blob 都可擷取。 如果您只想要在特定虛擬目錄為 Blob 編製索引，您可以使用選擇性的**查詢**參數指定該目錄。

如何使用 [REST API](/rest/api/searchservice/create-data-source) \(部分內容為機器翻譯\) 建立 Blob 資料來源物件的範例：

```
POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
    "name" : "blob-datasource",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "ResourceId=/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name/;" },
    "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
}   
```

### <a name="4---create-the-index"></a>4 - 建立索引

索引會指定文件、屬性和其他建構中可形塑搜尋體驗的欄位。

以下說明如何使用可搜尋的 `content` 欄位建立索引，以儲存從 blob 擷取的文字︰   

```http
    POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }
```

如需建立索引的詳細資訊，請參閱[建立索引](/rest/api/searchservice/create-index)

### <a name="5---create-the-indexer"></a>5 - 建立索引子

索引子會以目標搜尋索引連接資料來源，並提供排程來自動重新整理資料。

建立索引和資料來源之後，就可以開始建立索引子。

Blob 索引子的索引子定義範例：

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }
```

這個索引子每隔兩小時就會執行一次 (已將排程間隔設為 "PT2H")。 若每隔 30 分鐘就要執行索引子，可將間隔設為 "PT30M"。 支援的最短間隔為 5 分鐘。 排程為選擇性 - 如果省略，索引子只會在建立時執行一次。 不過，您隨時都可依需求執行索引子。   

如需建立索引子 API 的詳細資訊，請參閱 [建立索引子](/rest/api/searchservice/create-indexer)。

如需有關定義索引子排程的詳細資訊，請參閱[如何排程 Azure 認知搜尋的索引子](search-howto-schedule-indexers.md)。

## <a name="accessing-secure-data-in-storage-accounts"></a>存取儲存體帳戶中的安全資料

使用防火牆和虛擬網路可以進一步保護 Azure 儲存體帳戶。 如果您想要從 blob 儲存體帳戶或使用防火牆或虛擬網路保護的 Data Lake Gen2 儲存體帳戶來索引內容，請遵循透過「信任的 [服務」例外狀況安全地存取儲存體帳戶中資料](search-indexer-howto-access-trusted-service-exception.md)的指示。

## <a name="see-also"></a>另請參閱

深入了解 Azure 儲存體索引子：

* [Azure Blob 索引子](search-howto-indexing-azure-blob-storage.md)
* [Azure Data Lake Storage Gen2 索引子](search-howto-index-azure-data-lake-storage.md)
* [Azure 資料表索引子](search-howto-indexing-azure-tables.md)