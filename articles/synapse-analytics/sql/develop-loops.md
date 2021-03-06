---
title: 使用 T-sql 迴圈
description: 在 Synapse SQL 中使用 SQL 集區來使用 T-sql 迴圈、取代資料指標，以及開發相關解決方案的秘訣。
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 33e1ebc2269ef1db6bb0646f845b09be1a01c724
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91289050"
---
# <a name="use-t-sql-loops-in-synapse-sql"></a>在 Synapse SQL 中使用 T-sql 迴圈
本文提供您在 Synapse SQL 中使用 T-sql 迴圈、取代資料指標，以及使用 SQL 集區開發相關解決方案的基本秘訣。

## <a name="purpose-of-while-loops"></a>WHILE 迴圈的用途

Synapse SQL 支援 [WHILE](https://docs.microsoft.com/sql/t-sql/language-elements/while-transact-sql?view=sql-server-ver15&preserve-view=true) 迴圈，以重複執行語句區塊。 只要指定的條件都成立，或者在程式碼使用 BREAK 關鍵字特別終止迴圈之前，這個 WHILE 迴圈都會繼續下去。 

SQL 集區中的迴圈適用于取代 SQL 程式碼中定義的資料指標。 幸運的是，幾乎所有以 SQL 程式碼撰寫的資料指標都是向前快轉，並且只讀取多樣性。 因此，WHILE 迴圈是取代資料指標的絕佳替代方案。

## <a name="replace-cursors-in-sql-pool"></a>取代 SQL 集區中的資料指標

在深入探討之前，請先考慮下列問題：「是否可以重寫此資料指標以使用以集合為基礎的作業？」 在許多情況下，答案是肯定的，且通常是最好的方法。 以集合為基礎的作業執行速度通常會比反復的逐列方法更快。

向前快轉的唯讀資料指標很容易以迴圈結構取代。 下列程式碼是一個簡單的範例。 程式碼範例會更新資料庫中每個資料表的統計資料。 藉由反覆迴圈中的資料表，每個命令就能依序執行。

首先，建立暫存資料表，其中包含用來識別個別陳述式的唯一資料列數目：

```sql
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

其次，初始化執行迴圈所需的變數：

```sql
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

現在每次對一個陳數式執行一次迴圈：

```sql
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

最後，將第一個步驟中建立的暫存資料表卸除

```sql
DROP TABLE #tbl;
```

## <a name="next-steps"></a>後續步驟

如需更多開發秘訣，請參閱[開發概觀](develop-overview.md)。
