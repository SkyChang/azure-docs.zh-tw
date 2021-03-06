---
title: 在 Apache HBase 叢集中限定的 CPU-Azure HDInsight
description: 針對 Azure HDInsight 中 Apache HBase 叢集中區域伺服器上的限定 CPU 進行疑難排解
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: 16c994029e91d743f1c2a7e2eab51eb86fc378e8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "75887303"
---
# <a name="scenario-pegged-cpu-on-region-server-in-apache-hbase-cluster-in-azure-hdinsight"></a>案例： Azure HDInsight 中的 Apache HBase 叢集中的區域伺服器上已限定 CPU

本文說明與 Azure HDInsight 叢集互動時，問題的疑難排解步驟和可能的解決方式。

## <a name="issue"></a>問題

Apache HBase 區域伺服器進程的開始時間接近 200% CPU，導致 HBase Master 進程上引發警示，而叢集無法在完整容量運作。

## <a name="cause"></a>原因

如果您正在執行 HBase cluster 3.4 版，可能是因為將 jdk 升級為版本 1.7.0 _151 所造成的潛在錯誤。 我們看到的徵兆是，區域伺服器進程開始接近 200% CPU (若要確認這項作業，請執行 `top` 命令; 如果有接近 200% cpu 的進程，請取得其 pid，並藉由執行 ) 來確認其為區域伺服器進程 `ps -aux | grep` 。

## <a name="resolution"></a>解決方案

1. 在叢集中的所有節點上安裝 jdk 1.8，如下所示：

    * 執行腳本動作 `https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/upgradetojdk18allnodes.sh` 。 請務必選取要在所有節點上執行的選項。

    * 或者，您可以登入每個個別節點，然後執行命令 `sudo add-apt-repository ppa:openjdk-r/ppa -y && sudo apt-get -y update && sudo apt-get install -y openjdk-8-jdk` 。

1. 移至 Ambari UI- `https://<clusterdnsname>.azurehdinsight.net` 。

1. 流覽至 **HBase->的 [>advanced->advanced** ] `hbase-env configs` ，並將變數變更 `JAVA_HOME` 為 `export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64` 。 儲存配置變更。

1. [選用，但建議使用] [清除叢集中的所有資料表](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/)。

1. 再次從 Ambari UI 重新開機所有需要重新開機的 HBase 服務。

1. 視叢集上的資料而定，叢集可能需要幾分鐘的時間才能達到穩定狀態。 確認叢集達到穩定狀態的方式，就是檢查 HMaster UI (所有區域伺服器都應該是作用中) 從 Ambari (重新整理) 或從前端節點執行 HBase shell，然後執行狀態命令。

若要確認升級是否成功，請使用適當的 java 版本來檢查相關的 HBase 程式是否已啟動，例如區域伺服器檢查是否為：

```
ps -aux | grep regionserver, and verify the version like '''/usr/lib/jvm/java-8-openjdk-amd64/bin/java
```

## <a name="next-steps"></a>後續步驟

如果您沒有看到您的問題，或無法解決您的問題，請瀏覽下列其中一個管道以取得更多支援：

* 透過 [Azure 社群支援](https://azure.microsoft.com/support/community/)獲得由 Azure 專家所提供的解答。

* 與 [@AzureSupport](https://twitter.com/azuresupport) 聯繫 - 專為改善客戶體驗而設的官方 Microsoft Azure 帳戶，協助 Azure 社群連接至適當的資源，例如解答、支援及專家等。

* 如果需要更多協助，您可在 [Azure 入口網站](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/) 提交支援要求。 在功能表列選取 [支援] 或開啟 [說明 + 支援] 中心。 如需詳細資訊，請參閱[如何建立 Azure 支援要求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) (機器翻譯)。 您可透過 Microsoft Azure 訂用帳戶來存取訂用帳戶管理和帳單支援，並透過其中一項 [Azure 支援方案](https://azure.microsoft.com/support/plans/)以取得技術支援。
