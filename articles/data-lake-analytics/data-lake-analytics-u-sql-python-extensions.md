---
title: 在 Azure Data Lake Analytics 中使用 Python 擴充 U-SQL 指令碼
description: 了解如何使用 Azure Data Lake Analytics 以 U-SQL 指令碼執行 Python 程式碼
services: data-lake-analytics
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/20/2017
ms.custom: devx-track-python
ms.openlocfilehash: b15ab268433e4220d499f3e1fe7cb90ffac2a1be
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "87876012"
---
# <a name="extend-u-sql-scripts-with-python-code-in-azure-data-lake-analytics"></a>在 Azure Data Lake Analytics 中使用 Python 程式碼擴充 U-SQL 指令碼

## <a name="prerequisites"></a>必要條件

開始之前，請確定 Python 擴充功能已安裝在您的 Azure Data Lake Analytics 帳戶中。

* 在 Azure 入口網站中，瀏覽至您的 Data Lake Analytics 帳戶
* 在左窗格中，於 [入門]**** 中按一下 [範例指令碼]****
* 按一下 [安裝 U-SQL 擴充程式]****，然後按一下 [確定]****

## <a name="overview"></a>概觀

U-SQL 的 Python 擴充可讓開發人員進行大量的 Python 程式碼平行執行。 以下範例說明基本概念：

* 使用 `REFERENCE ASSEMBLY` 陳述式啟用 U-SQL 指令碼的 Python 延伸模組
* 使用 `REDUCE` 作業分割索引鍵上的輸入資料
* U-SQL 的 Python 延伸模組有內建的歸納器 (`Extension.Python.Reducer`)，可執行指派給歸納器之每一個頂點上的 Python 程式碼
* U-SQL 指令碼包含內嵌的 Python 程式碼，其中的 `usqlml_main` 函式會接受 pandas 資料框架作為輸入，並傳回 pandas 資料框架作為輸出。

```usql
REFERENCE ASSEMBLY [ExtPython];
DECLARE @myScript = @"
def get_mentions(tweet):
    return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )
def usqlml_main(df):
    del df['time']
    del df['author']
    df['mentions'] = df.tweet.apply(get_mentions)
    del df['tweet']
    return df
";
@t  =
    SELECT * FROM
       (VALUES
           ("D1","T1","A1","@foo Hello World @bar"),
           ("D2","T2","A2","@baz Hello World @beer")
       ) AS date, time, author, tweet );
@m  =
    REDUCE @t ON date
    PRODUCE date string, mentions string
    USING new Extension.Python.Reducer(pyScript:@myScript);
OUTPUT @m
    TO "/tweetmentions.csv"
    USING Outputters.Csv();
```

## <a name="how-python-integrates-with-u-sql"></a>Python 如何與 U-SQL 整合

### <a name="datatypes"></a>資料類型

* U-SQL 的字串和數值資料行在 Pandas 和 U-SQL 之間會如現狀轉換
* U-SQL 的 Null 與 Pandas 的 `NA` 值會互相轉換

### <a name="schemas"></a>結構描述

* U-SQL 不支援 Pandas 的索引向量。 Python 函式中所有的輸入資料框架一律具有 64 位元的數值索引，範圍從 0 到資料列數目減 1。
* U-SQL 資料集不能有重複的資料行名稱。
* U-SQL 資料集的資料行名稱不是字串。

### <a name="python-versions"></a>Python 版本

僅支援 Python 3.5.1 (針對 Windows 編譯)。

### <a name="standard-python-modules"></a>標準 Python 模組

包含所有的標準 Python 模組。

### <a name="additional-python-modules"></a>其他 Python 模組

除了標準 Python 程式庫，還包含數個常用的 Python 程式庫︰

* pandas
* numpy
* numexpr

### <a name="exception-messages"></a>例外狀況訊息

目前，Python 程式碼中的例外狀況是顯示為泛型頂點失敗。 在未來，U-SQL 作業的錯誤訊息將會顯示 Python 例外狀況訊息。

### <a name="input-and-output-size-limitations"></a>輸入和輸出的大小限制

指派給每個頂點的記憶體數量皆有上限。 目前，該限制為 6 GB 用於 AU。 因為輸入和輸出資料框架必須存在於Python 程式碼的記憶體中，輸入和輸出的大小總和不能超過 6 GB。

## <a name="next-steps"></a>後續步驟

* [Microsoft Azure Data Lake Analytics 概觀](data-lake-analytics-overview.md)
* [使用 Data Lake Tools for Visual Studio 開發 U-SQL 指令碼](data-lake-analytics-data-lake-tools-get-started.md)
* [針對 Azure 資料湖分析工作使用 U-SQL 視窗函式](data-lake-analytics-use-window-functions.md)
* [使用 Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
