---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 2604a1608f21d7239db755027e15b8198fb3f9f2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "81791661"
---
### <a name="functions-2x-and-higher"></a>Functions 2.x 和更新版本

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```

|屬性  |預設 | 描述 |
|---------|---------|---------|
|maxBatchSize|10|每個接收迴圈接收到的事件計數上限。|
|prefetchCount|300|基礎 `EventProcessorHost` 所使用的預設預先擷取計數。|
|batchCheckpointFrequency|1|要在建立 EventHub 資料指標檢查點之前處理的事件批次數目。|

> [!NOTE]
> 有關 Azure Functions 2.x 和更高版本中 host.json 的參考，請參閱[適用於 Azure Functions 的 host.json 參考](../articles/azure-functions/functions-host-json.md)。

### <a name="functions-1x"></a>Functions 1.x

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|屬性  |預設 | 描述 |
|---------|---------|---------| 
|maxBatchSize|64|每個接收迴圈接收到的事件計數上限。|
|prefetchCount|n/a|基礎 `EventProcessorHost` 將要使用的預設預先擷取。| 
|batchCheckpointFrequency|1|要在建立 EventHub 資料指標檢查點之前處理的事件批次數目。| 

> [!NOTE]
> 有關 Azure Functions 1.x 中 host.json 的參考，請參閱[適用於 Azure Functions 1.x 的 host.json 參考](../articles/azure-functions/functions-host-json-v1.md)。

