---
title: 彈性資料庫工具字彙
description: 彈性資料庫工具所用詞彙的解釋
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/04/2018
ms.openlocfilehash: 9c229fe6c1ccc7e2ce8e4f46ece51f315c8b7a90
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91362609"
---
# <a name="elastic-database-tools-glossary"></a>彈性資料庫工具字彙
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

下列詞彙是針對 [彈性資料庫工具](elastic-scale-introduction.md)所定義。 這些工具是用來管理[分區對應](elastic-scale-shard-map-management.md)，並且包含[用戶端程式庫](elastic-database-client-library.md)、[分割合併工具](elastic-scale-overview-split-and-merge.md)、[彈性集區](elastic-pool-overview.md)及[查詢](elastic-query-overview.md)。 

在[使用彈性資料庫工具加入分區](elastic-scale-add-a-shard.md)和[使用 RecoveryManager 類別來修正分區對應問題](elastic-database-recovery-manager.md)中會用到這些詞彙。

![彈性擴縮詞彙][1]

**資料庫**： Azure SQL Database 中的資料庫。 

**資料依存路由**：可讓應用程式根據特定的分區化索引鍵連接到分區的功能。 請參閱 [資料相依路由](elastic-scale-data-dependent-routing.md)。 請對照 **[Multi-Shard Query](elastic-scale-multishard-querying.md)**。

**全域分區對應**：在**分區集**之內，分區化索引鍵及其個別分區之間的對應。 全域分區對應儲存在 **分區對應管理員**中。 請對照 **本機分區對應**。

**清單分區對應**：逐一對應分區化索引鍵的分區對應。 請對照 **範圍分區對應**。   

**本機分區對應**：儲存在分區上的本機分區對應包含位於分區上之 Shardlet 的對應。

**多分區查詢**：能夠對多個分區發出查詢，使用 UNION ALL 語意傳回結果集 (也稱為「展開傳送查詢」)。 請對照 **資料相依路由**。

**多租用戶**和**單一租用戶**︰這會顯示單一租用戶資料庫和多租用戶資料庫︰

![單一和多租用戶資料庫](./media/elastic-scale-glossary/multi-single-simple.png)

以下是 **分區化** 單一和多租用戶資料庫的呈現。 

![單一和多租用戶資料庫](./media/elastic-scale-glossary/shards-single-multi.png)

**範圍分區對應**：根據多個連續值範圍來制訂分區分佈策略的分區對應。 

**參考資料表**：不分區化而是跨分區複寫的資料表。 例如，郵遞區號可以儲存參考資料表中。 

**分區**： Azure SQL Database 中的資料庫，可儲存來自分區化資料集的資料。 

**分區彈性**：能夠執行**水平縮放**與**垂直縮放**。

**分區化資料表**：分區化的資料表，亦即，資料根據其分區化索引鍵值而分佈至分區。 

**分區化索引鍵**：決定資料如何分佈至分區的資料行值。 值類型可以是下列其中一個：**int**、**bigint**、**varbinary** 或 **uniqueidentifier**。 

**分區集**：在分區對應管理員中屬於相同分區對應的分區集合。  

**Shardlet**：與分區上之分區化索引鍵的單一值相關聯的所有資料。 在重新分佈分區化資料表時，Shardlet 是可能的資料移動最小單位。 

**分區對應**：分區化索引鍵和其個別分區之間的對應集合。

**分區對應管理員**：管理物件和資料存放區，其中包含分區對應、分區位置，以及一或多個分區集的對應。

![圖表顯示與 shardmaps_global、shards_global 和 shard_mappings_global 相關聯的分區對應管理員。][2]

## <a name="verbs"></a>動詞
**水平縮放**：藉由在分區對應中新增或移除分區而相應放大 (或縮小) 分區集合的動作，如下所示。

![水平和垂直縮放][3]

**合併**：將兩個分區的 Shardlet 移至一個分區並相應地更新分區對應的動作。

**Shardlet 移動**：將單一 Shardlet 移至不同分區的動作。 

**分區**：根據分區化索引鍵將結構完全相同的資料水平分割至多個資料庫的動作。

**分割**：將一個分區的數個 Shardlet 移至另一個 (通常是新的) 分區的動作。 使用者提供分區化金鑰做為分割點。

**垂直縮放**：相應增加 (或減少) 個別分區之計算大小的動作。 例如，將分區從 Standard 變更為 Premium (獲得更多計算資源)。 

[!INCLUDE [elastic-scale-include](../../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/elastic-scale-glossary/glossary.png
[2]: ./media/elastic-scale-glossary/mappings.png
[3]: ./media/elastic-scale-glossary/h_versus_vert.png

