---
title: 對應資料流的效能和調整指南
description: 了解哪些關鍵因素會影響 Azure Data Factory 中對應資料流的效能。
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.custom: seo-lt-2019
ms.date: 08/12/2020
ms.openlocfilehash: a6f2c16730a9140fdbd1710a3aa0df0ee91795d6
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91874827"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>對應資料流的效能和調整指南

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory 中的對應資料流程可提供無程式碼介面，以大規模設計及執行資料轉換。 如果您不熟悉對應資料流，請參閱[對應資料流概觀](concepts-data-flow-overview.md)。 本文強調各種調整及優化資料流程的方式，讓它們符合您的效能基準。

觀看下列影片，以瞭解如何使用資料流程來轉換資料的一些取樣時間。

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4rNxM]

## <a name="testing-data-flow-logic"></a>測試資料流程邏輯

當您從 ADF UX 設計和測試資料流程時，debug 模式可讓您以互動方式測試即時 Spark 叢集。 這可讓您預覽資料並執行資料流程，而不需要等待叢集介入。 如需詳細資訊，請參閱[偵錯模式](concepts-data-flow-debug-mode.md)。

## <a name="monitoring-data-flow-performance"></a>監視資料流程效能

當您使用「偵測模式」來確認轉換邏輯之後，請以端對端的方式，以管線中的活動來執行您的資料流程。 資料流程會使用「 [執行資料流程」活動](control-flow-execute-data-flow-activity.md)在管線中實際運作。 相較于顯示轉換邏輯之詳細執行計畫和效能設定檔的其他 Azure Data Factory 活動，資料流程活動具有獨特的監視體驗。 若要查看資料流程的詳細監視資訊，請按一下管線活動執行輸出中的 [眼鏡] 圖示。 如需詳細資訊，請參閱[監視對應資料流](concepts-data-flow-monitoring.md)。

![資料流程監視](media/data-flow/monitoring-details.png "資料流程監視 2")

監視資料流程效能時，有四個可能的瓶頸需要注意：

* 叢集啟動時間
* 從來源讀取
* 轉換時間
* 寫入至接收 

![資料流程監視](media/data-flow/monitoring-performance.png "資料流程監視 3")

叢集啟動時間是加速 Apache Spark 叢集所需的時間。 此值位於 [監視] 畫面的右上角。 資料流程會在一次性的模型上執行，而每個作業都會使用隔離的叢集。 此啟動時間通常需要3-5 分鐘的時間。 針對連續作業，可以藉由啟用存留時間值來減少這種情況。 如需詳細資訊，請參閱 [優化 Azure Integration Runtime](#ir)。

資料流程利用 Spark 優化程式，在「階段」中重新排序並執行您的商務邏輯，以儘快執行。 針對您的資料流程所寫入的每個接收，監視輸出會列出每個轉換階段的持續時間，以及將資料寫入接收所需的時間。 最大的時間可能是資料流程的瓶頸。 如果最大的轉換階段包含來源，您可能會想要進一步優化讀取時間。 如果轉換花費很長的時間，您可能需要重新分割或增加整合執行時間的大小。 如果接收處理時間很大，您可能需要擴大您的資料庫，或確認您未輸出至單一檔案。

一旦識別出資料流程的瓶頸之後，請使用下列優化策略來改善效能。

## <a name="optimize-tab"></a>最佳化索引標籤

[ **優化** ] 索引標籤包含設定 Spark 叢集之資料分割配置的設定。 此索引標籤存在於資料流程的每個轉換中，並指定您是否要在轉換完成 **之後** 重新分割資料。 調整資料分割可讓您控制跨計算節點的資料分佈，以及對整體資料流程效能都有正面和負面影響的資料位置優化。

![螢幕擷取畫面顯示 [優化] 索引標籤，其中包含資料分割選項、資料分割類型和資料分割數目。](media/data-flow/optimize.png)

預設會選取 [ *使用目前* 的資料分割]，這會指示 Azure Data Factory 保留轉換的目前輸出資料分割。 當重新分割資料需要時間時，在大部分的情況下建議 *使用目前* 的資料分割。 您可能會想要重新分割資料的案例包括：大幅扭曲資料的匯總和聯結之後，或在 SQL DB 上使用來源資料分割。

若要變更任何轉換的資料分割，請選取 [ **優化** ] 索引標籤，然後選取 [ **設定資料分割** ] 選項按鈕。 您會看到一系列的資料分割選項。 資料分割的最佳方法會根據您的資料量、候選索引鍵、null 值和基數而有所不同。 

> [!IMPORTANT]
> 單一分割區會將所有分散式資料合併成單一資料分割。 這是非常慢的作業，也會對所有下游轉換和寫入造成顯著的影響。 Azure Data Factory 強烈建議您不要使用此選項，除非有明確的商務原因需要這麼做。

每個轉換都有下列資料分割選項可供使用：

### <a name="round-robin"></a>迴圈 

迴圈配置資源會將資料平均分散到資料分割。 當您沒有絕佳的關鍵候選項目來執行穩固的智慧型資料分割策略時，請使用迴圈配置資源。 您可以設定實體分割區的數目。

### <a name="hash"></a>雜湊

Azure Data Factory 會產生資料行的雜湊以產生統一的資料分割，讓具有類似值的資料列落在相同的資料分割中。 當您使用 Hash 選項時，請測試可能的資料分割誤差。 您可以設定實體分割區的數目。

### <a name="dynamic-range"></a>動態範圍

動態範圍會根據您提供的資料行或運算式來使用 Spark 動態範圍。 您可以設定實體分割區的數目。 

### <a name="fixed-range"></a>固定範圍

針對分割資料行內的值，建立可提供固定範圍的運算式。 若要避免資料分割扭曲，在使用此選項之前，您應該先對資料有充分的瞭解。 您為運算式輸入的值會當做資料分割函數的一部分使用。 您可以設定實體分割區的數目。

### <a name="key"></a>機碼

如果您對資料的基數有充分的瞭解，索引鍵分割可能是不錯的策略。 索引鍵分割會為數據行中的每個唯一值建立資料分割。 您無法設定分割區數目，因為此數目是根據資料中的唯一值。

> [!TIP]
> 手動設定資料分割配置 reshuffles 資料，並可抵消 Spark 優化工具的優點。 最佳做法是不要手動設定分割，除非您需要。

## <a name="optimizing-the-azure-integration-runtime"></a><a name="ir"></a> 優化 Azure Integration Runtime

資料流程會在執行時間啟動的 Spark 叢集上執行。 使用的叢集設定是在活動的整合執行時間 (IR) 中定義。 定義整合執行時間時，有三個要進行的效能考慮：叢集類型、叢集大小和存留時間。

如需如何建立 Integration Runtime 的詳細資訊，請參閱 [Azure Data Factory 中的 Integration Runtime](concepts-integration-runtime.md)。

### <a name="cluster-type"></a>叢集類型

Spark 叢集的類型有三個可用的選項： [一般用途]、[記憶體優化] 和 [計算優化]。

**一般用途** 叢集是預設選項，適用于大部分的資料流程工作負載。 這些通常是效能和成本的最佳平衡。

如果您的資料流程有許多聯結和查閱，您可能會想要使用 **記憶體優化** 的叢集。 記憶體優化叢集可以將更多資料儲存在記憶體中，並將您可能得到的記憶體不足錯誤降至最低。 記憶體優化的每個核心都有最高的價格點，但也傾向于更成功地產生管線。 如果您在執行資料流程時遇到記憶體不足的錯誤，請切換至記憶體優化的 Azure IR 設定。 

**計算優化** 不適合 ETL 工作流程，Azure Data Factory 團隊不建議用於大部分的生產工作負載。 針對較簡單、非記憶體密集的資料轉換（例如篩選資料或新增衍生的資料行），計算優化的叢集可依每個核心以較便宜的價格來使用。

### <a name="cluster-size"></a>叢集大小

資料流程會將資料處理散發到 Spark 叢集中的不同節點，以平行方式執行作業。 具有更多核心的 Spark 叢集會增加計算環境中的節點數目。 更多節點會增加資料流程的處理能力。 增加叢集的大小通常是減少處理時間的簡單方法。

預設叢集大小為四個驅動程式節點和四個背景工作節點。  當您處理更多資料時，建議使用較大的群集。 以下是可能的調整大小選項：

| 背景工作核心 | 驅動程式核心 | 核心總數 | 注意 |
| ------------ | ------------ | ----------- | ----- |
| 4 | 4 | 8 | 適用于計算優化 |
| 8 | 8 | 16 | |
| 16 | 16 | 32 | |
| 32 | 16 | 48 | |
| 64 | 16 | 80 | |
| 128 | 16 | 144 | |
| 256 | 16 | 272 | |

資料流程的定價是在 vcore 時，這表示叢集大小和執行時間因素都在此。 當您擴大時，每分鐘的叢集成本會增加，但整體時間將會降低。

> [!TIP]
> 叢集的大小會影響資料流程效能的程度會有上限。 視您的資料大小而定，增加叢集大小將會停止改善效能的一點。 例如，如果您有比資料分割更多的節點，新增其他節點將無法提供協助。 最佳做法是從小規模開始，並擴大以符合您的效能需求。 

### <a name="time-to-live"></a>存留時間

根據預設，每個資料流程活動都會根據 IR 設定來建立新的叢集。 叢集啟動時間需要幾分鐘的時間，而且資料處理在完成之前無法啟動。 如果您的管線包含多個 **連續** 資料流程，您可以啟用 (TTL) 值的存留時間。 指定存留時間值會讓叢集在其執行完成之後，維持一段特定時間的存留時間。 如果新的作業在 TTL 時間內開始使用 IR，它會重複使用現有的叢集，而啟動時間將會大幅降低。 第二個工作完成後，叢集將會再次維持在 TTL 時間的運作狀態。

一次只能在單一叢集上執行一項作業。 如果有可用的叢集，但兩個數據流開始，則只有一個會使用即時叢集。 第二個工作會啟動自己的隔離叢集。

如果大部分的資料流程都平行執行，則不建議您啟用 TTL。 

> [!NOTE]
> 使用自動解析整合執行時間時，無法使用存留時間

## <a name="optimizing-sources"></a>優化來源

針對 Azure SQL Database 以外的每個來源，建議您繼續 **使用目前** 的資料分割做為選取的值。 從所有其他來源系統讀取時，資料流程會根據資料的大小，將資料平均地分割。 每隔 128 MB 的資料就會建立一個新的資料分割。 當您的資料大小增加時，就會增加分割區數目。

任何自訂資料分割都會在 Spark 讀取資料 *之後* 發生，而且會對資料流程效能造成負面影響。 由於資料會在讀取時平均分割，因此不建議這麼做。 

> [!NOTE]
> 讀取速度可以受限於來源系統的輸送量。

### <a name="azure-sql-database-sources"></a>Azure SQL Database 來源

Azure SQL Database 有一個稱為「來源」資料分割的唯一資料分割選項。 啟用來源分割可以藉由在來源系統上啟用平行連線，來改善 Azure SQL DB 的讀取時間。 指定資料分割數目以及如何分割資料。 使用具有高基數的資料分割資料行。 您也可以輸入符合來源資料表之資料分割配置的查詢。

> [!TIP]
> 針對來源分割，SQL Server 的 i/o 是瓶頸。 新增過多的資料分割可能會讓源資料庫飽和。 使用這個選項時，通常會有四個或五個數據分割是理想的選擇。

![來源分割](media/data-flow/sourcepart3.png "來源分割")

#### <a name="isolation-level"></a>隔離等級

在 Azure SQL 來源系統上讀取的隔離等級會對效能造成影響。 選擇 [讀取未認可] 可提供最快的效能，並防止任何資料庫鎖定。 若要深入瞭解 SQL 隔離等級，請參閱 [瞭解隔離等級](https://docs.microsoft.com/sql/connect/jdbc/understanding-isolation-levels?view=sql-server-ver15)。

#### <a name="read-using-query"></a>使用查詢讀取

您可以使用資料表或 SQL 查詢來讀取 Azure SQL Database。 如果您要執行 SQL 查詢，查詢必須先完成，才能開始轉換。 SQL 查詢可能很適合用來推送執行速度較快的作業，並減少從 SQL Server （例如 SELECT、WHERE 和 JOIN 語句）讀取的資料量。 當您推送作業時，您將無法在資料進入資料流程之前，追蹤轉換的歷程和效能。

### <a name="azure-synapse-analytics-sources"></a>Azure Synapse Analytics 來源

使用 Azure Synapse Analytics 時，來源選項中會有一個稱為「 **啟用暫存** 」的設定。 這可讓 ADF 使用 [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-ver15)從 Synapse 讀取，以大幅改善讀取效能。 啟用 PolyBase 需要您在 [資料流程] 活動設定中指定 Azure Blob 儲存體或 Azure Data Lake Storage gen2 預備位置。

![啟用暫存](media/data-flow/enable-staging.png "啟用暫存")

### <a name="file-based-sources"></a>以檔案為基礎的來源

雖然資料流程支援多種檔案類型，但 Azure Data Factory 建議使用 Spark 原生 Parquet 格式，以獲得最佳的讀取和寫入時間。

如果您是在一組檔案上執行相同的資料流程，建議您從資料夾讀取，並使用萬用字元路徑或從檔案清單讀取。 單一資料流程活動執行可以在 batch 中處理所有檔案。 如需如何設定這些設定的詳細資訊，請參閱連接器檔，例如 [Azure Blob 儲存體](connector-azure-blob-storage.md#source-transformation)。

可能的話，請避免使用 For-Each 活動來執行一組檔案的資料流程。 這會導致每個的每個反復專案啟動其自己的 Spark 叢集，這通常不是必要的，而且可能很昂貴。 

## <a name="optimizing-sinks"></a>優化接收

當資料流程寫入至接收時，任何自訂資料分割都會在寫入之前立即發生。 就像來源一樣，在大部分情況下，建議您繼續 **使用目前** 的資料分割做為選取的資料分割選項。 即使您的目的地並未分割，資料分割資料的寫入速度會明顯比未分割的資料更快。 以下是各種接收類型的個別考慮。 

### <a name="azure-sql-database-sinks"></a>Azure SQL Database 接收

使用 Azure SQL Database 時，預設資料分割應該會在大部分情況下運作。 您的接收可能有太多資料分割可供您的 SQL database 處理。 如果您遇到這種情況，請減少 SQL Database 接收輸出的資料分割數目。

#### <a name="disabling-indexes-using-a-sql-script"></a>使用 SQL 腳本停用索引

在 SQL database 中的負載之前停用索引，可大幅提升寫入資料表的效能。 在寫入至您的 SQL 接收器之前，請先執行下列命令。

`ALTER INDEX ALL ON dbo.[Table Name] DISABLE`

寫入完成後，請使用下列命令重建索引：

`ALTER INDEX ALL ON dbo.[Table Name] REBUILD`

您可以使用 Azure SQL DB 中的前置和後置 SQL 腳本，或在對應資料流程中的 Synapse 接收，以原生方式來完成這些工作。

![停用索引](media/data-flow/disable-indexes-sql.png "停用索引")

> [!WARNING]
> 停用索引時，資料流程實際上會取得資料庫的控制權，而且查詢目前不可能成功。 如此一來，許多 ETL 作業都會在夜間觸發，以避免發生此衝突。 如需詳細資訊，請瞭解 [停用索引的條件約束](https://docs.microsoft.com/sql/relational-databases/indexes/disable-indexes-and-constraints?view=sql-server-ver15)

#### <a name="scaling-up-your-database"></a>擴大您的資料庫

在您的管線執行之前，您可以為來源及接收 Azure SQL DB 和 DW 排定調整大小作業來增加輸送量，並在達到 DTU 限制時將 Azure 節流降至最低。 當您的管線執行完成之後，請將您的資料庫大小調整回正常執行比率。

### <a name="azure-synapse-analytics-sinks"></a>Azure Synapse Analytics 接收

寫入至 Azure Synapse Analytics 時，請確定 [ **啟用預備** 環境] 設定為 [true]。 這可讓 ADF 使用 [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) 進行寫入，以大量地大量載入資料。 使用 PolyBase 時，您必須參考 Azure Data Lake Storage gen2 或 Azure Blob 儲存體帳戶來暫存資料。

除了 PolyBase 以外，相同的最佳作法也適用于 Azure Synapse Analytics 為 Azure SQL Database。

### <a name="file-based-sinks"></a>以檔案為基礎的接收 

雖然資料流程支援多種檔案類型，但 Azure Data Factory 建議使用 Spark 原生 Parquet 格式，以獲得最佳的讀取和寫入時間。

如果資料平均分佈， **使用目前** 的資料分割將是寫入檔案的最快分割選項。

#### <a name="file-name-options"></a>檔案名稱選項

寫入檔案時，您可以選擇每個都有效能影響的命名選項。

![接收選項](media/data-flow/file-sink-settings.png "接收選項")

選取 **預設** 選項將會寫入最快的速度。 每個分割區會等於具有 Spark 預設名稱的檔案。 如果您只是從資料的資料夾讀取，這會很有用。

設定命名 **模式** 會將每個分割檔重新命名為更容易使用的名稱。 這項作業會在寫入後發生，而且會比選擇預設值稍微慢一點。 每個分割區可讓您手動命名每個個別的資料分割。

如果資料行對應到您想要輸出資料的方式，您可以選取 [ **資料行] 中**的 [資料]。 這會 reshuffles 資料，而且如果資料行未平均分佈，可能會影響效能。

**輸出至單一** 檔案會將所有資料結合成單一資料分割。 這會導致較長的寫入時間，特別是針對大型資料集。 Azure Data Factory 團隊強烈建議您 **不要** 選擇這個選項，除非有明確的商業理由要這麼做。

### <a name="cosmosdb-sinks"></a>CosmosDB 接收器

寫入至 CosmosDB 時，在資料流程執行期間改變輸送量和批次大小可以改善效能。 這些變更只會在資料流程活動執行期間生效，並會在結束後返回原始的集合設定。 

**批次大小：** 計算資料的粗略資料列大小，並確保資料列大小 * 批次大小小於2000000。 如果是，請增加批次大小以取得更好的輸送量

**輸送量：** 在這裡設定較高的輸送量設定，可讓檔更快寫入 CosmosDB。 請記住，以高輸送量設定為基礎的較高 RU 成本。

**寫入輸送量預算：** 使用小於每分鐘總 ru 數的值。 如果您有大量 Spark 資料分割的資料流程，設定預算輸送量將可讓這些分割區進行更多的平衡。


## <a name="optimizing-transformations"></a>優化轉換

### <a name="optimizing-joins-exists-and-lookups"></a>優化聯結、存在和查閱

#### <a name="broadcasting"></a>廣播

在聯結、查閱和存在轉換中，如果其中一個或兩個數據流夠小，可納入背景工作節點記憶體，您可以藉由啟用 **廣播**來將效能優化。 廣播是指您將小型資料框架傳送到叢集中的所有節點。 這可讓 Spark 引擎執行聯結，而不需要重新輪換大型資料流程中的資料。 Spark 引擎預設會自動決定是否要廣播聯結的一端。 如果您熟悉傳入的資料，並知道某個資料流程將會明顯小於另一個串流，您可以選取 **固定** 的廣播。 固定廣播會強制 Spark 廣播選取的資料流程。 

如果廣播資料的大小對 Spark 節點而言太大，您可能會遇到記憶體不足的錯誤。 若要避免發生記憶體不足的錯誤，請使用 **記憶體優化** 的叢集。 如果您在資料流程執行期間遇到廣播超時，則可以關閉廣播優化。 不過，這會造成資料流程的執行速度變慢。

![聯結轉換最佳化](media/data-flow/joinoptimize.png "聯結最佳化")

#### <a name="cross-joins"></a>交叉聯結

如果您在聯結條件中使用常值，或在聯結的兩端有多個相符專案，則 Spark 會以交叉聯結的形式執行聯結。 交叉聯結是一個完整的笛卡兒乘積，可篩選出聯結的值。 這遠比其他聯結類型慢很多。 確定您在聯結條件的雙方都有資料行參考，以避免影響效能。

#### <a name="sorting-before-joins"></a>聯結之前的排序

不同於 SSIS 之類工具中的合併聯結，聯結轉換不是必要的合併聯結作業。 聯結索引鍵不需要在轉換之前進行排序。 Azure Data Factory 的小組不建議在對應的資料流程中使用排序轉換。

### <a name="window-transformation-performance"></a>視窗轉換效能

[視窗轉換](data-flow-window.md)會依據您在轉換設定中選取作為子句一部分的資料行中的值來分割您的資料 ```over()``` 。 在 Windows 轉換中會公開一些很受歡迎的匯總和分析函數。 但是，如果您的使用案例是為了排名或資料列數目的目的，在整個資料集上產生一個視窗 ```rank()``` ```rowNumber()``` ，建議您改為使用「 [排名」轉換](data-flow-rank.md) 和「 [代理索引鍵」轉換](data-flow-surrogate-key.md)。 這些轉換將會使用這些函式來執行更好的完整資料集作業。

### <a name="repartitioning-skewed-data"></a>重新分割扭曲的資料

某些轉換（例如聯結和匯總）重新您的資料分割區，有時可能會導致扭曲的資料。 扭曲的資料表示資料不會平均分散到資料分割。 高度扭曲的資料可能會導致下游轉換和接收寫入變慢。 您可以按一下監視顯示中的轉換，在資料流程執行中的任何時間點檢查資料的不對稱度。

![偏斜和峰](media/data-flow/skewness-kurtosis.png "偏斜和峰")

監視顯示器會顯示如何將資料分散到每個資料分割，以及兩個度量、偏斜和峰。 非**對稱**性是指非對稱資料的量值，而且可以有正數、零、負值或未定義的值。 負誤差表示左尾的長度超過右邊。 [**峰值**] 是資料是否為繁重或亮尾的量值。 不需要高峰值。 最理想的偏斜範圍介於-3 和3之間，而峰值範圍小於10。 解讀這些數位的簡單方式，就是查看分割區圖表，並查看1個橫條圖是否明顯大於其餘部分。

如果您的資料未在轉換後平均分割，您可以使用 [ [優化]](#optimize-tab) 索引標籤重新分割。 重新輪換資料需要一些時間，而且可能無法改善您的資料流程效能。

> [!TIP]
> 如果您重新分割資料，但有重新資料的下游轉換，請在用來當做聯結索引鍵的資料行上使用雜湊分割。

## <a name="using-data-flows-in-pipelines"></a>在管線中使用資料流程 

使用多個資料流程建立複雜的管線時，您的邏輯流程對時間和成本可能會有重大影響。 本節涵蓋不同架構策略的影響。

### <a name="executing-data-flows-in-parallel"></a>平行執行資料流程

如果您以平行方式執行多個資料流程，ADF 會為每個活動增加個別的 Spark 叢集。 這可讓每個工作以平行方式隔離及執行，但會導致多個叢集同時執行。

如果您的資料流程平行執行，建議您不要啟用 Azure IR 存留時間屬性，因為它會導致多個未使用的暖集區。

> [!TIP]
> 您可以在 data lake 中暫存您的資料，並使用萬用字元路徑來處理單一資料流程中的資料，而不是在每個活動的中多次執行相同的資料流程。

### <a name="execute-data-flows-sequentially"></a>依序執行資料流程

如果您依序執行資料流程活動，建議您在 Azure IR 設定中設定 TTL。 ADF 將重複使用計算資源，進而加快叢集的啟動時間。 每個活動仍會隔離，每次執行都會收到新的 Spark 內容。

依序執行工作可能會花費最長的時間來執行端對端，但會提供邏輯作業的明確分隔。

### <a name="overloading-a-single-data-flow"></a>多載單一資料流程

如果您將所有邏輯都放在單一資料流程中，ADF 將會在單一 Spark 實例上執行整個作業。 雖然這似乎是降低成本的一種方法，但它會將不同的邏輯流程混合在一起，而且可能很難監視和偵測。 如果某個元件失敗，則工作的其他所有部分也會失敗。 Azure Data Factory 團隊建議將資料流程組織成獨立的商務邏輯流程。 如果您的資料流程變得太大，將其分割成分開的元件將可簡化監視和偵錯工具。 雖然對資料流程中的轉換數目沒有硬性限制，但是有太多的作業會使工作變得複雜。

## <a name="next-steps"></a>後續步驟

請參閱其他與效能相關的資料流程文章：

- [資料流程活動](control-flow-execute-data-flow-activity.md)
- [監視資料流程效能](concepts-data-flow-monitoring.md)
