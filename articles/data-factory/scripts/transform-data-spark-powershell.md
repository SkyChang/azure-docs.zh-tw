---
title: 使用 PowerShell 在雲端中轉換資料
description: 這個 PowerShell 指令碼會藉由在 Azure HDInsight Spark 叢集上執行 Spark 程式，來轉換雲端中的資料。
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/12/2017
ms.openlocfilehash: c09d0532b845472d0ccaac1ad57e3772630bb5c9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "74932056"
---
# <a name="powershell-script---transform-data-in-cloud-using-azure-data-factory"></a>PowerShell 指令碼 - 使用 Azure Data Factory 轉換雲端中的資料

這個範例 PowerShell 指令碼會藉由在 Azure HDInsight Spark 叢集上執行 Spark 程式，來建立會轉換雲端中資料的管線。 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>必要條件
* **Azure 儲存體帳戶**。 您需要建立 python 指令碼和輸入檔案，並上傳至 Azure 儲存體。 spark 程式的輸出會儲存在這個儲存體帳戶中。 隨選 Spark 叢集與其主要儲存體是使用相同的儲存體帳戶。  

### <a name="upload-python-script-to-your-blob-storage-account"></a>將 python 指令碼上傳至 Blob 儲存體帳戶
1. 使用下列內容建立名為 **WordCount_Spark .py** 的 python 檔案： 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. 將** &lt; storageAccountName &gt; **取代為您 Azure 儲存體帳戶的名稱。 然後儲存檔案。 
3. 在 Azure Blob 儲存體中，建立名為 **adftutorial** 的容器 (如果不存在)。 
4. 建立名為 **spark** 的資料夾。
5. 在 **spark** 資料夾下，建立名為 **script** 的子資料夾。 
6. 將 **WordCount_Spark.py** 檔案上傳至 **script** 子資料夾。 


### <a name="upload-the-input-file"></a>上傳輸入檔案
1. 建立名為 **minecraftstory.txt** 的檔案並填入一些文字。 Spark 程式會計算這段文字中的字數。 
2. 在 blob 容器的 `spark` 資料夾中，建立名為 `inputfiles` 的子資料夾。 
3. 將 `minecraftstory.txt` 上傳至 `inputfiles` 子資料夾。 

## <a name="sample-script"></a>範例指令碼
> [!IMPORTANT]
> 這個指令碼會建立 JSON 檔案，該檔案定義硬碟上 c:\ 資料夾中的 Data Factory 實體 (已連結的服務、資料集和管線)。

[!code-powershell[main](../../../powershell_scripts/data-factory/transform-data-using-spark/transform-data-using-spark.ps1 "Transform data using Spark")]

## <a name="clean-up-deployment"></a>清除部署

執行範例指令碼之後，您可以使用下列命令以移除資源群組及與其相關聯的所有資源：

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```
若要從資源群組移除資料處理站，請執行下列命令： 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令：

| Command | 注意 |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | 建立用來存放所有資源的資源群組。 |
| [設定->set-azdatafactoryv2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | 建立資料處理站。 |
| [設定->set-azdatafactoryv2linkedservice](/powershell/module/az.datafactory/set-Azdatafactoryv2linkedservice) | 在資料處理站中建立已連結的服務。 已連結的服務會將資料存放區或計算連結到資料處理站。 |
| [設定->invoke-azdatafactoryv2pipeline](/powershell/module/az.datafactory/set-Azdatafactoryv2pipeline) | 在資料處理站中建立管線。 管線包含一或多個執行特定作業的活動。 在此管線中，Spark 活動會藉由在 Azure HDInsight Spark 叢集上執行程式，來轉換資料。 |
| [Invoke->invoke-azdatafactoryv2pipeline](/powershell/module/az.datafactory/invoke-Azdatafactoryv2pipeline) | 建立管線的執行。 也就是說，執行管線。 |
| [AzDataFactoryV2ActivityRun](/powershell/module/az.datafactory/get-Azdatafactoryv2activityrun) | 在管線中取得活動的執行 (活動執行) 的相關詳細資料。 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | 刪除資源群組，包括所有的巢狀資源。 |
|||

## <a name="next-steps"></a>後續步驟

如需有關 Azure PowerShell 的詳細資訊，請參閱 [Azure PowerShell 文件](https://docs.microsoft.com/powershell/)。

您可以在 [Azure Data Factory PowerShell 範例](../samples-powershell.md)中，找到其他 Azure Data Factory PowerShell 指令碼範例。
