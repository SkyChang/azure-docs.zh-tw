---
title: Azure Cosmos DB 常見的使用案例與情況
description: 了解 Azure Cosmos DB 的前五大使用案例：使用者產生的內容、事件記錄、目錄資料、使用者喜好設定資料，以及物聯網 (IoT)。
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 05/21/2019
ms.openlocfilehash: 76016da2ec542091aa57d5081e275a1f9f6671cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85114260"
---
# <a name="common-azure-cosmos-db-use-cases"></a>常見的 Azure Cosmos DB 使用案例
本文提供數個常見的 Azure Cosmos DB 使用案例概觀。  本文中的建議可作為使用 Cosmos DB 來開發應用程式時的起點。   

閱讀本文後，您將能夠回答下列問題： 

* Azure Cosmos DB 有哪些常見的使用案例？
* 將 Azure Cosmos DB 用於零售應用程式有哪些優點？
* 使用 Azure Cosmos DB 作為物聯網 (IoT) 系統的資料存放區有哪些優點？
* 將 Azure Cosmos DB 用於 Web 與行動應用程式有哪些優點？

## <a name="introduction"></a>簡介
[Azure Cosmos DB](../cosmos-db/introduction.md) 是 Microsoft 的全域分散式資料庫服務。 此服務的設計目的，是要讓客戶在任意數目的地理區域彈性地 (且獨立地) 調整輸送量和儲存空間。 Azure Cosmos DB 是現今市場中第一個提供涵蓋輸送量、延遲、可用性及一致性之全方位[服務等級協定](https://azure.microsoft.com/support/legal/sla/cosmos-db/)的全域分散式資料庫服務。 

Azure Cosmos DB 是一種全域分散式、多模型資料庫，廣泛用於各種應用程式和使用案例。 對於任何需要低毫秒級回應時間及需要快速且全域調整的[無伺服器](https://azure.com/serverless)應用程式來說，它是一個理想的選擇。 它以原生方式和可延伸的方式支援多個資料模型 (索引鍵/值、文件、圖形及單欄式) 和許多 API (包括[適用於 MongoDB 的 Azure Cosmos DB API](mongodb-introduction.md)、[SQL API](documentdb-introduction.md)、[Gremlin API](graph-introduction.md) 及[資料表 API](table-introduction.md)) 來進行資料存取。 

以下是 Azure Cosmos DB 的一些特性，可讓 Cosmos DB 非常適合用於有全域發展企圖的高效能應用程式。

* Azure Cosmos DB 會原生分割您的資料來達到高可用性和延展性。 針對所有單一區域帳戶和所有使用寬鬆一致性的多區域帳戶，Azure Cosmos DB 提供保證 99.99% 的可用性、輸送量、低延遲、一致性，並針對所有多區域資料庫帳戶提供 99.999% 的讀取可用性。
* Azure Cosmos DB 具有以 SSD 支持的儲存體，可提供低延遲的毫秒級回應時間。
* Azure Cosmos DB 支援最終、一致前置碼、工作階段及限定過期等一致性層級，因此能提供充分的彈性和高性價比。 沒有資料庫服務可在層級一致性上提供像 Azure Cosmos DB 一樣多的彈性。 
* Azure Cosmos DB 具有彈性的資料友善計價模型，可針對儲存體和輸送量單獨計價。
* Azure Cosmos DB 的保留輸送量模型可讓您以讀取/寫入數量，而非基礎硬體的 CPU/記憶體/IOPs 來作為思考方向。
* Azure Cosmos DB 的設計可讓您調整至大約每日數兆個要求的巨大要求量。

對於需要低回應時間，而且必須處理大量讀取和寫入的 Web 應用程式、行動應用程式、遊戲應用程式和 IoT 應用程式來說，這些特性很有助益。

## <a name="iot-and-telematics"></a>IoT 和遠距通訊
IoT 使用案例在如何內嵌、處理和儲存資料方面通常共用一些模式。  首先，這些系統需要內嵌於各種地區設定的裝置感應器中暴增的資料量。 接著，這些系統會處理並分析資料流資料，以衍生即時的資訊分析。 然後將資料封存到冷儲存體，進行批次分析。 Microsoft Azure 提供可適用于 IoT 使用案例的豐富服務，包括 Azure Cosmos DB、Azure 事件中樞、Azure 串流分析、Azure 通知中樞、Azure Machine Learning、Azure HDInsight 和 Power BI。 

:::image type="content" source="./media/use-cases/iot.png" alt-text="Azure Cosmos DB IoT 參考架構" border="false":::

Azure 事件中樞可以擷取暴增的資料量，因為它提供高輸送量資料擷取和低延遲。 您可以將需要處理以取得即時資訊分析的內嵌資料，使用漏斗方式倒入 Azure 串流分析以進行即時分析。 您可以將資料載入 Azure Cosmos DB 以進行臨機操作查詢。 將資料載入 Azure Cosmos DB 之後，這些資料便可供查詢。 此外，可以在變更摘要上讀取新的資料以及現有資料的變更。 變更摘要是永久性、僅附加記錄，會依序將變更儲存至 Cosmos 容器。 Azure Cosmos DB 中的全部資料或資料變更可用來在即時分析中作為參考資料。 此外，您還可以將 Azure Cosmos DB 資料連線到 HDInsight 來進行 Pig、Hive 或 Map/Reduce 作業，以進一步精簡和處理資料。  接著，系統會將精簡過的資料載入回 Azure Cosmos DB 以供報告使用。   

如需使用 Azure Cosmos DB、EventHubs 和 Storm 的範例 IoT 解決方案，請參閱 [GitHub 上的 hdinsight-storm-examples 存放庫 (英文)](https://github.com/hdinsight/hdinsight-storm-examples/)。

如需有關適用於 IoT 的 Azure 供應項目詳細資訊，請參閱 [建立您的物聯網](https://www.microsoft.com/en-us/internet-of-things)。 

## <a name="retail-and-marketing"></a>零售和行銷
Azure Cosmos DB 廣泛用於 Microsoft 自己的電子商務平台，這些平台執行了 Windows 市集和 XBox Live。 Azure Cosmos DB 也在零售業用於儲存目錄資料，並用於訂單處理管線中事件來源。

目錄資料使用方式案例涉及儲存和查詢一組實體屬性，例如人員、地點和產品。 目錄資料的一些範例包括使用者帳戶、產品目錄、IoT 裝置註冊及材料表系統。 這項資料的屬性可能會有所不同，而且可以隨時間變更，以符合應用程式需求。

請細想汽車零件供應商產品目錄的範例。 除了所有零件共用的通用屬性外，每個零件還可能會有自己的屬性。 此外，用於特定零件的屬性可能在明年推出新的模型時變更。 Azure Cosmos DB 支援彈性結構描述和階層式資料，因此很適合用來儲存產品目錄資料。

:::image type="content" source="./media/use-cases/product-catalog.png" alt-text="Azure Cosmos DB IoT 參考架構" border="false":::

Azure Cosmos DB 通常用於事件來源，以其[變更摘要](change-feed.md)功能推動事件取向架構。 變更摘要讓下游微服務能夠確實累加讀取和插入對 Azure Cosmos DB 進行的更新 (例如訂單事件)。 這項功能可以用來提供持續性事件存放區，以作為許多微服務 (可以實作為[無伺服器 Azure Functions](https://azure.com/serverless)) 之間狀態變更事件和訂單處理工作流程的訊息代理程式。

:::image type="content" source="./media/use-cases/event-sourcing.png" alt-text="Azure Cosmos DB IoT 參考架構" border="false":::

此外，儲存在 Azure Cosmos DB 中的資料還可以與 HDInsight 整合，以透過 Apache Spark 作業進行巨量資料分析。 如需有關適用於 Azure Cosmos DB 的 Spark Connector 詳細資訊，請參閱 [使用 Cosmos DB 和 HDInsight 執行 Spark 作業](spark-connector.md)。

## <a name="gaming"></a>遊戲
資料庫層是遊戲應用程式的重要元件。 現今的遊戲會在行動/主控台用戶端進行圖形處理，但依賴雲端來提供自訂和個人化的內容，如遊戲中的統計資料、社交媒體整合和得分排行榜。 遊戲的讀取和寫入通常需要單一毫秒延遲，以在遊戲中提供更吸引人的體驗。 在新遊戲推出和功能更新期間，遊戲資料庫必須快速且要能處理大量激增的要求率。

Azure Cosmos DB 已獲得 [The Walking Dead: No Man's Land](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) ([Next Games](https://www.nextgames.com/) 所製作) 和 [Halo 5: Guardians](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/) 等遊戲採用。 Azure Cosmos DB 可為遊戲開發人員提供下列優點︰

* Azure Cosmos DB 可彈性調高或調降效能。 這可讓遊戲藉由單一 API 呼叫就能處理同一時間區區十幾名玩家到數百萬名玩家的更新設定檔和統計資料。
* Azure Cosmos DB 支援毫秒讀取和寫入，可避免在進行遊戲時發生任何延遲。
* Azure Cosmos DB 的自動索引編製可針對多個不同的屬性進行即時篩選，例如依玩家的內部玩家識別碼、GameCenter、Facebook、Google ID 找出玩家，或根據玩家的公會成員資格進行查詢。 不用建置複雜的索引或分區化基礎結構就可做到這些事。
* 透過彈性的結構描述更加輕易地實作社交功能，包括遊戲中的交談訊息、玩家的公會成員資格、已完成的挑戰、得分排行榜和社交關係圖。
* 由於 Azure Cosmos DB 是作為一個受控平台即服務 (PaaS)，因此只需最基本的設定和管理工作，就能快速重覆作業而能縮短上市時間。

:::image type="content" source="./media/use-cases/gaming.png" alt-text="Azure Cosmos DB IoT 參考架構" border="false":::

## <a name="web-and-mobile-applications"></a>Web 與行動應用程式
Azure Cosmos DB 常用於 Web 與行動應用程式，而且適合用於建立社交互動模型、與第三方服務整合及建置豐富的個人化體驗。 Cosmos DB SDK 可用來建置使用熱門 [Xamarin 架構](mobile-apps-with-xamarin.md)的豐富 iOS 和 Android 應用程式。  

### <a name="social-applications"></a>社交應用程式
其中一個常見的 Azure Cosmos DB 使用案例，就是針對網路、行動和社交媒體應用程式儲存和查詢使用者產生的內容 (UGC)。 一些 UGC 範例包括對談、推文、部落格文章、評等和註解。 通常，社交媒體應用程式中的 UGC 會結合使用自由格式文字、屬性、標記和不受固定結構限制的關聯性。 聊天、評論及貼文之類的內容可以儲存在 Cosmos DB 中，而不須向關聯式對應層要求轉換或複雜物件。  開發人員在反覆查看應用程式程式碼時，可以輕鬆地新增或修改資料屬性以符合需求，進而加快開發的速度。  

與第三方社交網路整合的應用程式必須回應這些網路不斷變更的結構描述。 由於在 Cosmos DB 中預設會自動為資料編製索引，因此資料隨時可供查詢。 因此，這些應用程式可以有彈性地根據其各自的需求擷取投影。

許多社交應用程式是以全球的規模運作，而且可能出現無法預期的使用模式。 調整資料存放區的彈性十分重要，因為應用程式層會進行調整以符合使用需求。  您可以透過在 Cosmos DB 帳戶下新增其他資料分割區來相應放大。  此外，您也可以跨多個區域建立其他 Cosmos DB 帳戶。 如需了解 Cosmos DB 服務區域可用性，請參閱 [Azure 區域](https://azure.microsoft.com/regions/#services)。

:::image type="content" source="./media/use-cases/apps-with-global-reach.png" alt-text="Azure Cosmos DB IoT 參考架構" border="false":::

### <a name="personalization"></a>個人化
現今，現代應用程式具備複雜的檢視和體驗。 這些是通常是動態的，根據使用者喜好設定或情緒和品牌需求量身訂做。 因此，應用程式必須要能夠有效地擷取個人化設定，以快速呈現 UI 項目和體驗。 

JSON (Cosmos DB 所支援的格式) 是一種呈現 UI 配置資料的有效格式，因為它不只輕量，還可由 JavaScript 輕鬆解譯。 Cosmos DB 提供可微調的一致性層級，可允許快速讀取及低延遲寫入。 因此，將 UI 配置資料 (包括個人化設定) 以 JSON 文件形式儲存在 Cosmos DB 中，是跨不同線路取得這項資料的有效方法。

:::image type="content" source="./media/use-cases/personalization.png" alt-text="Azure Cosmos DB IoT 參考架構" border="false":::

## <a name="next-steps"></a>後續步驟

* 若要開始使用 Azure Cosmos DB，請依照我們的[快速入門](create-sql-api-dotnet.md)操作，這會逐步引導您建立帳戶及開始使用 Cosmos DB。

* 如果您想要閱讀有關使用 Azure Cosmos DB 的客戶詳細資訊，請參閱 [客戶案例研究](https://azure.microsoft.com/case-studies/?service=cosmos-db) 頁面。
