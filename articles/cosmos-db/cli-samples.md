---
title: 適用於 Azure Cosmos DB 的 Azure CLI 範例
description: 適用於 Azure Cosmos DB 的 Azure CLI 範例
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: 954215f04525e850151fdad93af6e7272b41b3df
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498458"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>適用於 Azure Cosmos DB 的 Azure CLI 範例

下表包含適用於 Azure Cosmos DB 之範例 Azure CLI 指令碼的連結。 使用右側的連結瀏覽至 API 特定範例。 所有 API 的通用範例皆相同。 您可以在 [Azure CLI 參考](/cli/azure/cosmosdb)中取得所有 Azure Cosmos DB CLI 命令的參考頁面。 您也可以在 [Azure Cosmos DB CLI GitHub 存放庫](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到 Azure Cosmos DB CLI 指令碼範例。

這些範例需要 Azure CLI 2.9.1 或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI](/cli/azure/install-azure-cli)

## <a name="common-samples"></a>通用範例

這些範例適用於所有 Azure Cosmos DB API

|Task | 描述 |
|---|---|
| [新增或容錯移轉區域](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | 新增區域、變更容錯移轉優先順序、觸發手動容錯移轉。|
| [帳戶金鑰和連接字串](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | 列出帳戶金鑰、唯讀金鑰、重新產生金鑰及列出連接字串。|
| [使用 IP 防火牆保護安全](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| 建立已設定 IP 防火牆的 Cosmos 帳戶。|
| [使用服務端點保護新的帳戶](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| 建立 Cosmos 帳戶並使用服務端點來保護其安全。|
| [使用服務端點保護現有帳戶](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| 終於設定好子網路後，將 Cosmos 帳戶更新為使用服務端點來保護其安全。|
|||

## <a name="core-sql-api-samples"></a>Core (SQL) API 範例

|Task | 描述 |
|---|---|
| [建立 Azure Cosmos 帳戶、資料庫和容器](scripts/cli/sql/create.md?toc=%2fcli%2fazure%2ftoc.json)| 建立適用於 Core (SQL) API 的 Azure Cosmos DB 帳戶、資料庫和容器。 |
| [建立具有自動調整功能的 Azure Cosmos 帳戶、資料庫和容器](scripts/cli/sql/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| 針對 Core (SQL) API，建立具有自動調整功能的 Azure Cosmos DB 帳戶、資料庫和容器。 |
| [變更輸送量](scripts/cli/sql/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | 更新資料庫和容器的 RU/秒。|
| [鎖定資源避免刪除](scripts/cli/sql/lock.md?toc=%2fcli%2fazure%2ftoc.json)| 使用資源鎖定避免刪除資源。|
|||

## <a name="mongodb-api-samples"></a>MongoDB API 範例

|Task | 描述 |
|---|---|
| [建立 Azure Cosmos 帳戶、資料庫和集合](scripts/cli/mongodb/create.md?toc=%2fcli%2fazure%2ftoc.json)| 建立適用於 MongoDB API 的 Azure Cosmos DB 帳戶、資料庫和集合。 |
| [建立具有自動調整功能及二個具有共用輸送量之集合的 Azure Cosmos 帳戶、資料庫和集合](scripts/cli/mongodb/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| 針對 MongoDB API，建立具有自動調整功能及二個具有共用輸送量之集合的 Azure Cosmos DB 帳戶和資料庫。 |
| [變更輸送量](scripts/cli/mongodb/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | 更新資料庫或集合的 RU/秒。|
| [鎖定資源避免刪除](scripts/cli/mongodb/lock.md?toc=%2fcli%2fazure%2ftoc.json)| 使用資源鎖定避免刪除資源。|
|||

## <a name="cassandra-api-samples"></a>Cassandra API 範例

|Task | 描述 |
|---|---|
| [建立 Azure Cosmos 帳戶、Keyspace 和資料表](scripts/cli/cassandra/create.md?toc=%2fcli%2fazure%2ftoc.json)| 建立適用於 Cassandra API 的 Azure Cosmos DB 帳戶、Keyspace 和資料表。 |
| [建立具有自動調整功能的 Azure Cosmos 帳戶、keyspace 和資料表](scripts/cli/cassandra/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| 針對 Cassandra API，建立具有自動調整功能的 Azure Cosmos DB 帳戶、Keyspace 和資料表。 |
| [變更輸送量](scripts/cli/cassandra/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | 更新 Keyspace 和資料表上的 RU/秒。|
| [鎖定資源避免刪除](scripts/cli/cassandra/lock.md?toc=%2fcli%2fazure%2ftoc.json)| 使用資源鎖定避免刪除資源。|
|||

## <a name="gremlin-api-samples"></a>Gremlin API 範例

|Task | 描述 |
|---|---|
| [建立 Azure Cosmos 帳戶、資料庫和圖表](scripts/cli/gremlin/create.md?toc=%2fcli%2fazure%2ftoc.json)| 建立 Gremlin API 的 Azure Cosmos DB 帳戶、資料庫和圖形。 |
| [建立具有自動調整功能的 Azure Cosmos 帳戶、資料庫和圖表](scripts/cli/gremlin/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| 針對 Gremlin API，建立具有自動調整功能的 Azure Cosmos DB 帳戶、資料庫和圖表。 |
| [變更輸送量](scripts/cli/gremlin/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | 更新資料庫和圖形的 RU/秒。|
| [鎖定資源避免刪除](scripts/cli/gremlin/lock.md?toc=%2fcli%2fazure%2ftoc.json)| 使用資源鎖定避免刪除資源。|
|||

## <a name="table-api-samples"></a>資料表 API 範例

|Task | 描述 |
|---|---|
| [建立 Azure Cosmos 帳戶和資料表](scripts/cli/table/create.md?toc=%2fcli%2fazure%2ftoc.json)| 建立資料表 API 的 Azure Cosmos DB 帳戶和資料表。 |
| [建立具有自動調整功能的 Azure Cosmos 帳戶和資料表](scripts/cli/table/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| 針對資料表 API，建立具有自動調整功能的 Azure Cosmos DB 帳戶和資料表。 |
| [變更輸送量](scripts/cli/table/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | 更新資料表的 RU/秒。|
| [鎖定資源避免刪除](scripts/cli/table/lock.md?toc=%2fcli%2fazure%2ftoc.json)| 使用資源鎖定避免刪除資源。|
|||
