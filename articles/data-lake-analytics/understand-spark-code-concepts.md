---
title: 瞭解 Azure Data Lake Analytics U-SQL 開發人員 Apache Spark 程式碼的概念。
description: 本文描述 Apache Spark 的概念，以協助 SQL 開發人員瞭解 Spark 程式碼的概念。
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: how-to
ms.custom: Understand-apache-spark-code-concepts
ms.date: 10/15/2019
ms.openlocfilehash: 7b5be20bb8b5eb1d56c1214104037d5d824445b3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "87132342"
---
# <a name="understand-apache-spark-code-for-u-sql-developers"></a>瞭解適用于 SQL 開發人員的 Apache Spark 程式碼

本節提供將 U SQL 腳本轉換成 Apache Spark 的高階指引。

- 它會從 [兩種語言的處理範例開始比較](#understand-the-u-sql-and-spark-language-and-processing-paradigms)
- 提供有關如何：
   - [轉換腳本](#transform-u-sql-scripts)（包括 U SQL 的資料列[集運算式](#transform-u-sql-rowset-expressions-and-sql-based-scalar-expressions)）
   - [.NET 程式碼](#transform-net-code)
   - [資料類型](#transform-typed-values)
   - [目錄物件](#transform-u-sql-catalog-objects)。

## <a name="understand-the-u-sql-and-spark-language-and-processing-paradigms"></a>瞭解 U SQL 和 Spark 語言和處理範例

開始將 Azure Data Lake Analytics ' U-SQL 腳本遷移至 Spark 之前，請先瞭解這兩個系統的一般語言和處理原理。

Sql-dmo 是類似 SQL 的宣告式查詢語言，可使用資料流程架構，並可讓您輕鬆地內嵌和相應放大以 .NET (撰寫的使用者程式碼，例如 c # ) 、Python 和 R。使用者擴充可以執行簡單的運算式或使用者定義函式，但是也可以讓使用者能夠實作為使用者定義的運算子，以執行自訂運算子來執行資料列集層級轉換、提取和寫入輸出。

Spark 是在 Scala、JAVA、Python、.NET 等中提供數種語言系結的向外延展架構。您主要以上述其中一種語言撰寫程式碼、建立稱為彈性分散式資料集的資料抽象概念 (RDD) 、資料框架和資料集，然後使用類似 LINQ 的網域特定語言 (DSL) 轉換它們。 它也提供 SparkSQL 做為資料框架和資料集抽象上的宣告式子語言。 DSL 提供兩種類別的作業、轉換和動作。 將轉換套用至資料抽象層將不會執行轉換，而是會建立將以動作提交以進行評估的執行計畫 (例如，將結果寫入臨時表或檔案，或列印結果) 。

因此，將 U SQL 腳本轉譯成 Spark 程式時，您必須決定要使用哪一種語言，至少產生資料框架抽象 (目前最常使用的資料抽象) ，以及您是否想要使用 DSL 或 SparkSQL 撰寫宣告式資料流程轉換。 在更複雜的情況下，您可能需要將您的 U SQL 腳本分割成一系列 Spark，以及使用 Azure Batch 或 Azure Functions 所執行的其他步驟。

此外，Azure Data Lake Analytics 在無伺服器作業服務環境中提供了內部 SQL，而 Azure Databricks 和 Azure HDInsight 都是以叢集服務的形式提供 Spark。 轉換您的應用程式時，您必須考慮現在建立、調整大小、調整和解除委任叢集的含意。

## <a name="transform-u-sql-scripts"></a>轉換 U-SQL 腳本

U SQL 腳本會遵循下列處理模式：

1. 資料會從非結構化檔案讀取、使用 `EXTRACT` 語句、位置或檔案集規格，以及內建或使用者定義的解壓縮者和所需的架構，或從 U SQL 資料表 (managed 或外部資料表) 。 它會以資料列集表示。
2. 資料列集會在多個會將 U SQL 運算式套用至資料列集，並產生新資料列集的 U SQL 語句中轉換。
3. 最後，產生的資料列集會使用語句來輸出到其中一個檔案， `OUTPUT` 該語句會指定 (s) 的位置，以及內建或使用者定義的輸出器，或是在 U SQL 資料表中。

腳本會延遲進行評估，這表示每個解壓縮和轉換步驟都會組成運算式樹狀架構，並在資料流程)  (全域評估。

Spark 程式很類似，您可以使用 Spark 連接器讀取資料並建立資料框架，然後使用類似 LINQ 的 DSL 或 SparkSQL 在資料框架上套用轉換，然後將結果寫入檔案、暫存 Spark 資料表、某些程式設計語言類型或主控台。

## <a name="transform-net-code"></a>轉換 .NET 程式碼

SQL-DMO 的運算式語言是 c #，它提供各種不同的方式來向外擴充自訂 .NET 程式碼。

因為 Spark 目前不支援執行 .NET 程式碼，所以您必須將運算式重寫為相等的 Spark、Scala、JAVA 或 Python 運算式，或找出呼叫 .NET 程式碼的方法。 如果您的腳本使用 .NET 程式庫，您可以選擇下列選項：

- 將您的 .NET 程式碼轉譯為 Scala 或 Python。
- 將您的 U SQL 腳本分割成幾個步驟，您可以在其中使用 Azure Batch 進程來套用 .NET 轉換 (如果您可以取得可接受的調整) 
- 使用稱為 Moebius 的開放原始碼中提供的 .NET 語言系結。 此專案不是處於支援的狀態。

在任何情況下，如果您的 U SQL 腳本中有大量的 .NET 邏輯，請透過您的 Microsoft 帳戶代表聯絡我們，以取得進一步的指引。

下列詳細資料適用于 .NET 的不同案例和 U SQL 腳本中的 c # 用法。

### <a name="transform-scalar-inline-u-sql-c-expressions"></a>轉換純量內嵌 U-SQL c # 運算式

U SQL 的運算式語言是 c #。 許多純量內嵌 U SQL 運算式都會以原生方式實作為改善效能，而更複雜的運算式可透過呼叫 .NET framework 來執行。

Spark 有自己的純量運算式語言 (是 DSL 或 SparkSQL) 的一部分，並可讓您呼叫以其裝載語言撰寫的使用者定義函式。

如果您在 SQL-DMO 中有純量運算式，您應該先找出最適當的原生瞭解 Spark 純量運算式來取得最大效能，然後將其他運算式對應到您所選 Spark 裝載語言的使用者定義函數。

請注意，.NET 和 c # 的類型語義不同于 Spark 裝載語言和 Spark 的 DSL。 如需類型系統差異的詳細資訊，請參閱 [下文](#transform-typed-values) 。

### <a name="transform-user-defined-scalar-net-functions-and-user-defined-aggregators"></a>轉換使用者定義的純量 .NET 函式和使用者定義匯總工具

U SQL 提供了呼叫任意純量 .NET 函式的方式，以及呼叫以 .NET 撰寫的使用者定義匯總工具。

Spark 也支援以大部分的裝載語言撰寫的使用者定義函式和使用者定義匯總工具，可從 Spark 的 DSL 和 SparkSQL 呼叫。

### <a name="transform-user-defined-operators-udos"></a>將使用者定義的運算子轉換 (Udo) 

SQL-DMO 提供數種使用者定義運算子類別 (Udo) ，例如擷取器、輸出器、歸納器、處理器、套用器和結合器，可以在 .NET (和-在 Python 和 R) 的某些範圍中撰寫。

Spark 沒有為運算子提供相同的擴充性模型，但有一些對等的功能。

與擷取器和輸出器的 Spark 相當於 Spark 連接器。 針對許多的擷取器，您可能會在 Spark 社區中找到對等的連接器。 針對其他人，您將必須撰寫自訂連接器。 如果 U SQL 解壓縮程式很複雜，而且使用了數個 .NET 程式庫，最好是在 Scala 中建立連接器，以使用 interop 呼叫 .NET 程式庫來執行資料的實際處理。 在此情況下，您必須將 .NET Core 執行時間部署至 Spark 叢集，並確認參考的 .NET 程式庫符合 .NET Standard 2.0。

您必須使用使用者定義函數和匯總工具，以及語義適當的 Spark DLS 或 SparkSQL 運算式來重寫其他類型的 Udo。 例如，您可以將處理器對應到各種 UDF 調用的選取，封裝為接受資料框架做為引數並傳回資料框架的函式。

### <a name="transform-u-sqls-optional-libraries"></a>轉換 U SQL 的選用程式庫

SQL-DMO 提供一組選擇性和示範程式庫，可提供 [Python](data-lake-analytics-u-sql-python-extensions.md)、 [R](data-lake-analytics-u-sql-r-extensions.md)、 [JSON、XML、AVRO 支援](https://github.com/Azure/usql/tree/master/Examples/DataFormats)和一些 [認知服務功能](data-lake-analytics-u-sql-cognitive.md)。

Spark 分別提供自己的 Python 和 R 整合、pySpark 和 SparkR，並提供可讀取和寫入 JSON、XML 和 AVRO 的連接器。

如果您需要轉換參考認知服務程式庫的腳本，我們建議您透過 Microsoft 客戶代表來與我們聯繫。

## <a name="transform-typed-values"></a>轉換具類型的值

因為 U SQL 的型別系統是以 .NET 型別系統為基礎，而 Spark 有自己的類型系統，會受到主機語言系結的影響，所以您必須確定您正在操作的型別已關閉，而針對某些類型，類型範圍、有效位數和/或小數位數可能稍微不同。 此外，U SQL 和 Spark 會 `null` 以不同的方式來處理值。

### <a name="data-types"></a>資料類型

下表提供 Spark、Scala 和 PySpark 中指定之 U SQL 類型的對等類型。

| U-SQL | Spark |  Scala | PySpark |
| ------ | ------ | ------ | ------ |
|`byte`       ||||
|`sbyte`      |`ByteType` |`Byte` | `ByteType`|
|`int`        |`IntegerType` |`Int` | `IntegerType`|
|`uint`       ||||
|`long`       |`LongType` |`Long` | `LongType`|
|`ulong`      ||||
|`float`      |`FloatType` |`Float` | `FloatType`|
|`double`     |`DoubleType` |`Double` | `DoubleType`|
|`decimal`    |`DecimalType` |`java.math.BigDecimal` | `DecimalType`|
|`short`      |`ShortType` |`Short` | `ShortType`|
|`ushort`     ||||
|`char`   | |`Char`||
|`string` |`StringType` |`String` |`StringType` |
|`DateTime`   |`DateType`, `TimestampType` |`java.sql.Date`, `java.sql.Timestamp` | `DateType`, `TimestampType`|
|`bool`   |`BooleanType` |`Boolean` | `BooleanType`|
|`Guid`   ||||
|`byte[]` |`BinaryType` |`Array[Byte]` | `BinaryType`|
|`SQL.MAP<K,V>`   |`MapType(keyType, valueType, valueContainsNull)` |`scala.collection.Map` | `MapType(keyType, valueType, valueContainsNull=True)`|
|`SQL.ARRAY<T>`   |`ArrayType(elementType, containsNull)` |`scala.collection.Seq` | `ArrayType(elementType, containsNull=True)`|

如需詳細資訊，請參閱

- [sql-dmo. .sql. 類型](https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.types.package)
- [Spark SQL 和資料框架類型](https://spark.apache.org/docs/latest/sql-ref-datatypes.html)
- [Scala 數值型別](https://www.scala-lang.org/api/current/scala/AnyVal.html)
- [pyspark](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#module-pyspark.sql.types)

### <a name="treatment-of-null"></a>Null 的處理

在 Spark 中，每個預設的類型在 U SQL 中都允許 Null 值，您可以明確地將純量非物件標示為可為 null。 Spark 可讓您將資料行定義為不可為 null，而不會強制執行條件約束，而且 [可能會導致錯誤的結果](https://medium.com/@weshoffman/apache-spark-parquet-and-troublesome-nulls-28712b06f836)。

在 Spark 中，Null 表示值不明。 Spark Null 值與任何值（包括其本身）不同。 兩個 Spark Null 值或 Null 值與其他任何值之間的比較會傳回 unknown，因為每個 Null 的值都是未知的。  

這種行為與 SQL-DMO 不同，它會遵循 c # 語義，其中 `null` 與任何值不同，但等於本身。  

因此 `SELECT` `WHERE column_name = NULL` ，即使在中有 Null 值，使用的 SparkSQL 語句也會傳回零個數據列 `column_name` ，而在 U SQL 中，它會傳回設定為的資料列 `column_name` `null` 。 同樣地， `SELECT` `WHERE column_name != NULL` 如果中有非 null 值，則使用的 Spark 語句會傳回零個數據列 `column_name` ，而在 U SQL 中，則會傳回具有非 null 的資料列。 因此，如果您想要使用 SQL-DMO 和 isnotnull，請分別使用[isnull](https://spark.apache.org/docs/2.3.0/api/sql/index.html#isnull)和[isnotnull](https://spark.apache.org/docs/2.3.0/api/sql/index.html#isnotnull) (或其 DSL 對等) 。

## <a name="transform-u-sql-catalog-objects"></a>轉換 U SQL 目錄物件

其中一個主要差異在於，您可以利用多個 SQL 腳本來利用它的目錄物件，其中許多都沒有直接的 Spark 對等專案。

Spark 提供 Hive 中繼存放區概念的支援，主要是資料庫和資料表，因此您可以將 SQL 資料庫和架構對應至 Hive 資料庫，並將雙 SQL 資料表對應至 Spark 資料表 (查看如何 [移動儲存在 U sql 資料表中的資料](understand-spark-data-formats.md#move-data-stored-in-u-sql-tables)) ，但不支援視圖、資料表值函式 (tvf) 、預存程式、U SQL 元件、外部資料源等。

您可以透過 Spark 中的程式碼函式和程式庫來建立 Tvf、預存程式和元件等的雙 SQL 程式碼物件模型，並使用主機語言的函式和程式性抽象機器制加以參考 (例如，透過匯入 Python 模組或參考 Scala 函數) 。

如果使用了 U SQL 目錄來跨專案和小組共用資料和程式碼物件，則必須使用共用的相等機制 (例如，) 共用程式碼物件的 Maven。

## <a name="transform-u-sql-rowset-expressions-and-sql-based-scalar-expressions"></a>轉換 U SQL 資料列集運算式和以 SQL 為基礎的純量運算式

SQL-DMO 的核心語言正在轉換資料列集，並以 SQL 為基礎。 以下是在 U-SQL 中最常用的資料列集運算式的非完整清單：

- `SELECT`/`FROM`/`WHERE`/`GROUP BY`+ 匯總 +`HAVING`/`ORDER BY`+`FETCH`
- `INNER`/`OUTER`/`CROSS`/`SEMI``JOIN`運算式
- `CROSS`/`OUTER``APPLY`運算式
- `PIVOT`/`UNPIVOT` 表達 式
- `VALUES` 資料列集函式

- 設定運算式 `UNION`/`OUTER UNION`/`INTERSECT`/`EXCEPT`

此外，U SQL 提供各種 SQL 型純量運算式，例如

- `OVER` 視窗化運算式
- 各種內建的匯總工具和排名函式 (`SUM` `FIRST` 等 ) 
- 一些最熟悉的 SQL 純量運算式： `CASE` 、 `LIKE` 、 (`NOT`) `IN` 、 `AND` 等等 `OR` 。

Spark 針對大部分的運算式在其 DSL 和 SparkSQL 表單中提供對等的運算式。 某些在 Spark 中不支援的運算式，必須使用原生 Spark 運算式和語義相等模式的組合進行重寫。 例如， `OUTER UNION` 必須將轉換成投射和等位的對等組合。

由於不同的 Null 值處理方式，如果所比較的兩個數據行都包含 Null 值，則在 Spark 中的聯結將永遠符合資料列，而 Spark 中的聯結將不符合這類資料行，除非加入明確的 Null 檢查。

## <a name="transform-other-u-sql-concepts"></a>轉換其他的 U SQL 概念

SQL-DMO 也提供各種不同的功能和概念，例如針對 SQL Server 資料庫、參數、純量和 lambda 運算式變數、系統變數、提示進行同盟查詢 `OPTION` 。

### <a name="federated-queries-against-sql-server-databasesexternal-tables"></a>針對 SQL Server 資料庫/外部資料表的同盟查詢

SQL-DMO 會提供資料來源和外部資料表，以及針對 Azure SQL Database 的直接查詢。 雖然 Spark 不提供相同的物件抽象概念，但它會為可用來查詢 SQL 資料庫的 [Azure SQL Database 提供 Spark 連接器](../azure-sql/database/spark-connector.md) 。

### <a name="u-sql-parameters-and-variables"></a>U-SQL 參數和變數

參數和使用者變數在 Spark 中具有對等的概念及其裝載語言。

例如，在 Scala 中，您可以使用關鍵字定義變數 `var` ：

```
var x = 2 * 3;
println(x)
```

從) 開始的 SQL 系統變數 (變數 `@@` 可以分為兩種類別：

- 可設定為特定值的可設定系統變數，以影響腳本行為
- 查詢系統和作業層級資訊的資訊系統變數

大部分可設定的系統變數在 Spark 中都沒有直接對等專案。 您可以透過將資訊當作引數傳遞至工作執行期間的引數來建立模型的部分資訊系統變數，有些則可能在 Spark 的裝載語言中有同等的函式。

### <a name="u-sql-hints"></a>U-SQL 提示

U SQL 提供數種語法方式來提供查詢最佳化工具和執行引擎的提示：  

- 設定 U SQL 系統變數
- `OPTION`與資料列集運算式相關聯的子句，以提供資料或計畫提示
- 聯結運算式語法中的聯結提示 (例如， `BROADCASTLEFT`) 

Spark 以成本為基礎的查詢最佳化工具有自己的功能，可提供提示及微調查詢效能。 請參閱對應的檔。

## <a name="next-steps"></a>接下來的步驟

- [瞭解適用于 U SQL 開發人員的 Spark 資料格式](understand-spark-data-formats.md)
- [適用於 Apache Spark 的 .NET](https://docs.microsoft.com/dotnet/spark/what-is-apache-spark-dotnet)
- [將您的巨量資料分析解決方案從 Azure Data Lake Storage Gen1 升級為 Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-upgrade.md)
- [使用 Azure Data Factory 中的 Spark 活動轉換資料](../data-factory/transform-data-using-spark.md)
- [使用 Azure Data Factory 中的 Hadoop Hive 活動轉換資料](../data-factory/transform-data-using-hadoop-hive.md)
- [什麼是 Azure HDInsight 中的 Apache Spark](../hdinsight/spark/apache-spark-overview.md)
