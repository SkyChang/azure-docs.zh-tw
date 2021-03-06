---
title: 建議的效能基準測試-Azure NetApp Files
description: 深入瞭解使用 Azure NetApp Files 進行大量效能和計量的基準測試建議。
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: conceptual
ms.date: 08/07/2019
ms.openlocfilehash: cf25ef59bc1ea5db61dcfb3c76c0d978cb1f95d0
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91931831"
---
# <a name="performance-benchmark-test-recommendations-for-azure-netapp-files"></a>適用於Azure NetApp Files 的效能基準測試建議

本文針對使用 Azure NetApp Files 的磁片區效能和計量提供基準測試建議。

## <a name="overview"></a>概觀

若要瞭解 Azure NetApp Files 磁片區的效能特性，您可以使用開放原始碼工具 [FIO](https://github.com/axboe/fio) 來執行一連串的基準測試，以模擬各種不同的工作負載。 FIO 可以安裝在 Linux 和 Windows 作業系統上。  它是一項絕佳的工具，可取得磁片區的 IOPS 和輸送量的快速快照。

### <a name="vm-instance-sizing"></a>VM 實例大小調整

為了獲得最佳結果，請確定您使用的虛擬機器 (VM) 實例已適當調整大小，以執行測試。 下列範例會使用 Standard_D32s_v3 實例。 如需 VM 實例大小的詳細資訊，請參閱適用于 Windows Vm 的 [azure 中的 windows 虛擬機器大小](../virtual-machines/sizes.md?toc=%252fazure%252fvirtual-network%252ftoc.json) ，以及 [azure 中 linux 虛擬機器的大小](../virtual-machines/sizes.md?toc=%252fazure%252fvirtual-machines%252flinux%252ftoc.json) （適用于 linux 型 vm）。

### <a name="azure-netapp-files-volume-sizing"></a>Azure NetApp Files 磁片區大小調整

確定您針對預期的效能層級選擇正確的服務等級和磁片區配額大小。 如需詳細資訊，請參閱 [Azure NetApp Files 的服務等級](azure-netapp-files-service-levels.md) 。

### <a name="virtual-network-vnet-recommendations"></a>虛擬網路 (VNet) 建議

您應該在與 Azure NetApp Files 相同的 VNet 中執行基準測試。 下列範例示範建議：

![VNet 建議](../media/azure-netapp-files/azure-netapp-files-benchmark-testing-vnet.png)

## <a name="installation-of-fio"></a>安裝 FIO

FIO 適用于 Linux 和 Windows 的二進位格式。 遵循 [FIO](https://github.com/axboe/fio) 中的 [二進位封裝] 區段，針對您選擇的平臺進行安裝。

## <a name="fio-examples-for-iops"></a>IOPS 的 FIO 範例 

本節中的 FIO 範例會使用下列設定：
* VM 實例大小： D32s_v3
* 容量集區服務層級和大小： Premium/50 TiB
* 磁片區配額大小： 48 TiB

下列範例顯示 FIO 隨機讀取和寫入。

### <a name="fio-8k-block-size-100-random-reads"></a>FIO：8k 區塊大小100% 隨機讀取

`fio --name=8krandomreads --rw=randread --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128 --size=4G --runtime=600 --group_reporting`

### <a name="output-68k-read-iops-displayed"></a>輸出：68k 讀取 IOPS 顯示

`Starting 4 processes`  
`Jobs: 4 (f=4): [r(4)][84.4%][r=537MiB/s,w=0KiB/s][r=68.8k,w=0 IOPS][eta 00m:05s]`

### <a name="fio-8k-block-size-100-random-writes"></a>FIO：8k 區塊大小100% 隨機寫入

`fio --name=8krandomwrites --rw=randwrite --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-73k-write-iops-displayed"></a>輸出：顯示73k 寫入 IOPS

`Starting 4 processes`  
`Jobs: 4 (f=4): [w(4)][26.7%][r=0KiB/s,w=571MiB/s][r=0,w=73.0k IOPS][eta 00m:22s]`

## <a name="fio-examples-for-bandwidth"></a>頻寬的 FIO 範例

本章節中的範例會顯示 FIO 順序的讀取和寫入。

### <a name="fio-64k-block-size-100-sequential-reads"></a>FIO：64k 區塊大小100% 連續讀取

`fio --name=64kseqreads --rw=read --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-118-gbits-throughput-displayed"></a>輸出：顯示 11.8 Gb/秒的輸送量

`Starting 4 processes`  
`Jobs: 4 (f=4): [R(4)][40.0%][r=1313MiB/s,w=0KiB/s][r=21.0k,w=0 IOPS][eta 00m:09s]`

### <a name="fio-64k-block-size-100-sequential-writes"></a>FIO：64k 區塊大小100% 順序寫入

`fio --name=64kseqwrites --rw=write --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

### <a name="output-122-gbits-throughput-displayed"></a>輸出：顯示 12.2 Gb/秒的輸送量

`Starting 4 processes`  
`Jobs: 4 (f=4): [W(4)][85.7%][r=0KiB/s,w=1356MiB/s][r=0,w=21.7k IOPS][eta 00m:02s]`

## <a name="volume-metrics"></a>磁片區計量

Azure NetApp Files 效能資料可透過 Azure 監視器的計數器取得。 您可以透過 Azure 入口網站取得計數器，並 REST API 取得要求。 

您可以查看下列資訊的歷程記錄資料：
* 讀取延遲的平均值 
* 寫入延遲的平均值 
* 讀取 IOPS (平均) 
* 寫入 IOPS (平均) 
* 磁片區邏輯大小 (平均) 
* 磁片區快照集大小 (平均) 

### <a name="using-azure-monitor"></a>使用 Azure 監視器 

您可以從 [計量] 頁面以每個磁片區為基礎來存取 Azure NetApp Files 計數器，如下所示：

![Azure 監視器計量](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-metrics.png)

您也可以在 Azure NetApp Files 的 Azure 監視器中建立儀表板，方法是前往 [計量] 頁面、對 NetApp 進行篩選，以及指定感興趣的磁片區計數器： 

![Azure 監視器儀表板](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-dashboard.png)

### <a name="azure-monitor-api-access"></a>Azure 監視器 API 存取

您可以使用 REST API 呼叫來存取 Azure NetApp Files 計數器。 請參閱 [Azure 監視器： Microsoft. NetApp/netAppAccounts/capacityPools/磁片區的支援計量](../azure-monitor/platform/metrics-supported.md#microsoftnetappnetappaccountscapacitypoolsvolumes) （適用于容量集區和磁片區的計數器）。

下列範例顯示用來查看邏輯磁片區大小的 GET URL：

`#get ANF volume usage`  
`curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/ANFACCOUNTGOESHERE/capacityPools/ANFPOOLGOESHERE/Volumes/ANFVOLUMEGOESHERE/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=VolumeLogicalSize`


## <a name="next-steps"></a>後續步驟

- [Azure NetApp Files 的服務等級](azure-netapp-files-service-levels.md)
- [Linux 的效能評定](performance-benchmarks-linux.md)