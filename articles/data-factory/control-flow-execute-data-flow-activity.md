---
title: 資料流程活動
description: 如何從 data factory 管線內部執行資料流程。
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: makromer
ms.date: 04/30/2020
ms.openlocfilehash: 5593b0d633b133c8a8295634b674218d5e6c6daf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89485032"
---
# <a name="data-flow-activity-in-azure-data-factory"></a>Azure Data Factory 中的資料流程活動

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

使用資料流程活動，透過對應資料流程來轉換和移動資料。 如果您不熟悉資料流程，請參閱 [對應資料流程總覽](concepts-data-flow-overview.md)

## <a name="syntax"></a>語法

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "MyDataFlow",
         "type": "DataFlowReference"
      },
      "compute": {
         "coreCount": 8,
         "computeType": "General"
      },
      "staging": {
          "linkedService": {
              "referenceName": "MyStagingLinkedService",
              "type": "LinkedServiceReference"
          },
          "folderPath": "my-container/my-folder"
      },
      "integrationRuntime": {
          "referenceName": "MyDataFlowIntegrationRuntime",
          "type": "IntegrationRuntimeReference"
      }
}

```

## <a name="type-properties"></a>類型屬性

屬性 | 描述 | 允許的值 | 必要
-------- | ----------- | -------------- | --------
資料流程 | 正在執行之資料流程的參考 | DataFlowReference | 是
integrationRuntime | 執行資料流程的計算環境。 如果未指定，將會使用自動解析 Azure integration runtime。 | IntegrationRuntimeReference | 否
compute. coreCount | Spark 叢集中使用的核心數目。 只有在使用自動解析 Azure Integration runtime 時才能指定 | 8、16、32、48、80、144、272 | 否
compute. computeType | Spark 叢集中使用的計算類型。 只有在使用自動解析 Azure Integration runtime 時才能指定 | "General"、"ComputeOptimized"、"MemoryOptimized" | 否
暫存. linkedService | 如果您使用 Azure Synapse Analytics 來源或接收，則用於 PolyBase 暫存的儲存體帳戶 | LinkedServiceReference | 只有當資料流程讀取或寫入 Azure Synapse Analytics
暫存. folderPath | 如果您使用 Azure Synapse Analytics 來源或接收器，則為用於 PolyBase 暫存的 blob 儲存體帳戶中的資料夾路徑 | 字串 | 只有當資料流程讀取或寫入 Azure Synapse Analytics

![執行資料流程](media/data-flow/activity-data-flow.png "執行資料流程")

### <a name="dynamically-size-data-flow-compute-at-runtime"></a>在執行時間動態調整資料流程計算的大小

您可以動態設定核心計數和計算類型屬性，以便在執行時間調整為傳入的來源資料大小。 使用查閱或取得中繼資料等管線活動，以找出源資料集資料的大小。 然後，在資料流程活動屬性中使用 [加入動態內容]。

![動態資料流程](media/data-flow/dyna1.png "動態資料流")

[以下是說明這項技術的簡短影片教學課程](https://www.youtube.com/watch?v=jWSkJdtiJNM)

### <a name="data-flow-integration-runtime"></a>資料流程整合執行時間

選擇要用於「資料流程」活動執行的 Integration Runtime。 根據預設，Data Factory 會使用自動解析的 Azure 整合執行時間搭配四個背景工作角色，而不會有存留時間 (TTL) 。 此 IR 具有一般目的計算類型，而且會在與您的 factory 相同的區域中執行。 您可以建立自己的 Azure 整合執行時間，為您的資料流程活動執行定義特定區域、計算類型、核心計數和 TTL。

針對管線執行，叢集是一個作業叢集，這需要幾分鐘的時間才會開始執行。 如果未指定 TTL，則每次執行管線時都需要此啟動時間。 如果您指定 TTL，暖叢集集區會在上一次執行之後指定的時間保持作用中狀態，因而縮短啟動時間。 例如，如果您有60分鐘的 TTL，並每隔一小時執行一次資料流程，叢集集區將會保持作用中狀態。 如需詳細資訊，請參閱 [Azure integration runtime](concepts-integration-runtime.md)。

![Azure Integration Runtime](media/data-flow/ir-new.png "Azure Integration Runtime")

> [!IMPORTANT]
> 資料流程活動中的 Integration Runtime 選取專案只適用于已 *觸發* 的管線執行。 使用資料流程來對管線進行偵錯工具，可在 debug 會話中指定的叢集上執行。

### <a name="polybase"></a>PolyBase

如果您使用 Azure Synapse Analytics (先前的 SQL 資料倉儲) 作為接收或來源，則必須為您的 PolyBase 批次負載選擇預備位置。 PolyBase 允許大量載入批次，而不是逐列載入資料。 PolyBase 大幅減少了 Azure Synapse Analytics 的載入時間。

## <a name="parameterizing-data-flows"></a>參數化資料流程

### <a name="parameterized-datasets"></a>參數化資料集

如果您的資料流程使用參數化資料集，請在 [ **設定** ] 索引標籤中設定參數值。

![執行資料流程參數](media/data-flow/params.png "參數")

### <a name="parameterized-data-flows"></a>參數化資料流程

如果您的資料流程已參數化，請在 [ **參數** ] 索引標籤中設定資料流程參數的動態值。您可以使用 ADF 管線運算式語言，或使用資料流程運算式語言來指派動態或常值參數值。 如需詳細資訊，請參閱 [資料流程參數](parameters-data-flow.md)。

### <a name="parameterized-compute-properties"></a>參數化計算屬性。

如果您使用自動解析 Azure Integration runtime 並指定 coreCount 和 compute. computeType 的值，您可以將核心計數或計算類型參數化。

![執行資料流程參數範例](media/data-flow/parameterize-compute.png "參數範例")

## <a name="pipeline-debug-of-data-flow-activity"></a>資料流程活動的管線偵錯工具

若要執行具有資料流程活動的 debug 管線執行，您必須透過頂端列上的資料流程 **調試** 滑杆來切換資料流程的偵測模式。 偵錯工具模式可讓您針對作用中的 Spark 叢集執行資料流程。 如需詳細資訊，請參閱[偵錯模式](concepts-data-flow-debug-mode.md)。

![偵錯按鈕](media/data-flow/debugbutton.png "偵錯按鈕")

Debug 管線會針對使用中的 debug 叢集執行，而不是針對資料流程活動設定中指定的整合執行時間環境。 啟動「偵測」模式時，您可以選擇 [debug compute] 環境。

## <a name="monitoring-the-data-flow-activity"></a>監視資料流程活動

資料流程活動具有特殊的監視體驗，您可以在其中查看分割、階段時間和資料歷程資訊。 在 [ **動作**] 底下，透過眼鏡圖示開啟 [監視] 窗格。 如需詳細資訊，請參閱 [監視資料流程](concepts-data-flow-monitoring.md)。

### <a name="use-data-flow-activity-results-in-a-subsequent-activity"></a>在後續活動中使用資料流程活動結果

資料流程活動會輸出有關寫入每個接收的資料列數目和從每個來源讀取之資料列的度量。 這些結果會在 `output` 活動執行結果的區段中傳回。 傳回的度量格式為下列 json 格式。

``` json
{
    "runStatus": {
        "metrics": {
            "<your sink name1>": {
                "rowsWritten": <number of rows written>,
                "sinkProcessingTime": <sink processing time in ms>,
                "sources": {
                    "<your source name1>": {
                        "rowsRead": <number of rows read>
                    },
                    "<your source name2>": {
                        "rowsRead": <number of rows read>
                    },
                    ...
                }
            },
            "<your sink name2>": {
                ...
            },
            ...
        }
    }
}
```

例如，若要在名為 ' dataflowActivity ' 的活動中，取得寫入至名為 ' sink1 ' 之接收器的資料列數目，請使用 `@activity('dataflowActivity').output.runStatus.metrics.sink1.rowsWritten` 。

若要從該接收中使用的名稱為 ' source1.rc ' 的來源取得讀取的資料列數目，請使用 `@activity('dataflowActivity').output.runStatus.metrics.sink1.sources.source1.rowsRead` 。

> [!NOTE]
> 如果接收的資料列數為零，則不會顯示在計量中。 您可以使用函式來驗證是否存在 `contains` 。 例如， `contains(activity('dataflowActivity').output.runStatus.metrics, 'sink1')` 會檢查是否有任何資料列寫入 sink1。

## <a name="next-steps"></a>後續步驟

請參閱 Data Factory 支援的控制流程活動： 

- [If 條件活動](control-flow-if-condition-activity.md)
- [執行管線活動](control-flow-execute-pipeline-activity.md)
- [For Each 活動](control-flow-for-each-activity.md)
- [取得中繼資料活動](control-flow-get-metadata-activity.md)
- [查閱活動](control-flow-lookup-activity.md)
- [Web 活動](control-flow-web-activity.md)
- [Until 活動](control-flow-until-activity.md)
