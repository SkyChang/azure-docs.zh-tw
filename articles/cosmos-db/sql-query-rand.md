---
title: Azure Cosmos DB 查詢語言中的 RAND
description: 瞭解 Azure Cosmos DB 中的 SQL 系統函數 RAND。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: ff098da778221868b0eddc17c426d2bf36eec0fe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "88794334"
---
# <a name="rand-azure-cosmos-db"></a>RAND (Azure Cosmos DB) 
 從 [0，1) 傳回隨機產生的數位值。
 
## <a name="syntax"></a>語法
  
```sql
RAND ()  
```  

## <a name="return-types"></a>傳回類型

  傳回數值運算式。

## <a name="remarks"></a>備註

  `RAND` 是非決定性函數。 的重複呼叫 `RAND` 不會傳回相同的結果。 這個系統函數將不會使用索引。


## <a name="examples"></a>範例
  
  下列範例會傳回隨機產生的數位值。
  
```sql
SELECT RAND() AS rand 
```  
  
 以下為結果集。  
  
```json
[{"rand": 0.87860053195618093}]  
``` 

## <a name="next-steps"></a>接下來的步驟

- [數學函數 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系統函數 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 簡介](introduction.md)
