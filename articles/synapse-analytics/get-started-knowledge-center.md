---
title: 教學課程：開始探索 Synapse 知識中心
description: 在本教學課程中，您將了解如何使用 Synapse 知識中心。
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 09/15/2020
ms.openlocfilehash: 47b8c45e22569cc758d00fb8534f409ecebf58ca
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2020
ms.locfileid: "91299926"
---
# <a name="explore-the-synapse-knowledge-center"></a>探索 Synapse 知識中心

在本教學課程中，您將了解如何使用 Synapse Studio 知識中心。

## <a name="getting-to-the-knowledge-center"></a>進入知識中心

有兩種方式可找到 Synapse Studio 中的知識中心：

  1. 在首頁中樞的實用連結底下，按一下名為**知識中心**的第一個連結。
  2. 在頂端的功能表列上，按一下 [？] 然後按一下 [知識中心]。

挑選其中一個方法並開啟 [知識中心]。

## <a name="overview"></a>概觀

**知識中心**可讓您執行三項動作：
* **立即使用範例**。 此選項已經過最佳化，可讓您盡快看到進行中的分析。 如果您需要 Synapse 運作方式的快速範例，請選擇此選項。
* **瀏覽可用範例**。 此選項可讓您連結資料集範例，並以 SQL 指令碼、筆記本和管線的形式新增程式碼範例。
* **導覽 Synapse Studio**。 此選項會帶您快速瀏覽 Synapse Studio 的基本組件。 如果您從未使用過 Synapse Studio，此選項會很實用。

## <a name="exploring-blob-storage-with-sql-on-demand"></a>使用隨選 SQL 探索 Blob 儲存體

1. 在**知識中心**中，按一下 [立即使用範例]
1. 選取 [使用 SQL 查詢來查詢資料] 
1. 按一下 [立即使用範例]
1. 其會建立新的 SQL 指令碼。
1. 捲動到第一個查詢 (第 28 行到第 32 行)，然後選取查詢文字
1. 按一下 [執行]。 其將會執行您選取的文字。

## <a name="loading-more-nyc-taxi-data"></a>載入更多 NYC 計程車資料
1. 在**知識中心**中，按一下 [瀏覽可用範例] 
1. 選取頂端的 [SQL 指令碼] 索引標籤
1. 選取 [載入紐約計程車資料集]
1. 在 [輸入] 底下，選擇 [選取現有集區]，然後選取 [SQLDB1]
1. 按一下 [開啟指令碼]
1. 新的 SQL 指令碼會隨即出現。
1. 按一下 [執行] 
1. 這會為所有 NYC 計程車資料建立數個資料表，並使用 T-SQL COPY 命令來載入這些資料表。

## <a name="next-steps"></a>後續步驟

* [開始使用 Azure Synapse Analytics](get-started.md)
* [建立工作區](quickstart-create-workspace.md)
* [使用 SQL 隨選](quickstart-sql-on-demand.md)
