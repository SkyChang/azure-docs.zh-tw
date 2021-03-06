---
title: 教學課程 - 使用 Azure CLI 管理 Azure 磁碟
description: 在本教學課程中，您會了解如何使用 Azure CLI 來建立及管理虛擬機器的 Azure 磁碟
author: cynthn
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.workload: infrastructure
ms.date: 08/20/2020
ms.author: cynthn
ms.custom: mvc, devx-track-azurecli
ms.subservice: disks
ms.openlocfilehash: 948a4ae8c329d69e404ef8d0f609748b955b0ecc
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2020
ms.locfileid: "89078844"
---
# <a name="tutorial---manage-azure-disks-with-the-azure-cli"></a>教學課程 - 使用 Azure CLI 管理 Azure 磁碟

Azure 虛擬機器 (VM) 使用磁碟來儲存作業系統、應用程式和資料。 建立 VM 時，請務必選擇適合所預期工作負載的磁碟大小和組態。 本教學課程說明如何部署及管理 VM 磁碟。 您將了解：

> [!div class="checklist"]
> * OS 磁碟和暫存磁碟
> * 資料磁碟
> * 標準和進階磁碟
> * 磁碟效能
> * 連結及準備資料磁碟
> * 磁碟快照集


## <a name="default-azure-disks"></a>預設 Azure 磁碟

建立 Azure 虛擬機器後，有兩個磁碟會自動連結到虛擬機器。

**作業系統磁碟** - 作業系統磁碟可裝載 VM 作業系統，其大小可以高達 2 TB。 OS 磁碟預設會標示為 /dev/sda  。 OS 磁碟的磁碟快取組態已針對 OS 效能進行最佳化。 因為此組態，OS 磁碟**不得**用於應用程式或資料。 請對應用程式和資料使用資料磁碟，本教學課程稍後會詳細說明。

**暫存磁碟** - 暫存磁碟會使用與 VM 位於相同 Azure 主機的固態磁碟機。 暫存磁碟的效能非常好，可用於暫存資料處理等作業。 不過，如果 VM 移至新的主機，則會移除儲存在暫存磁碟上的任何資料。 暫存磁碟的大小取決於 VM 大小。 暫存磁碟會標示為 /dev/sdb  ，其掛接點為 /mnt  。

## <a name="azure-data-disks"></a>Azure 資料磁碟

若要安裝應用程式和儲存資料，可以新增額外的資料磁碟。 資料磁碟應使用於任何需要持久且有回應之資料儲存體的情況。 虛擬機器的大小會決定可連結到 VM 的資料磁碟數目。

## <a name="vm-disk-types"></a>VM 磁碟類型

Azure 提供兩種類型的磁碟。

**標準磁碟** - 由 HDD 所支援，可提供符合成本效益的儲存體，同時保有效能。 標準磁碟適合用於具成本效益的開發和測試工作負載。

**進階磁碟** - 採用以 SSD 為基礎的高效能、低延遲磁碟。 最適合用於執行生產工作負載的 VM。 在[大小名稱](../vm-naming-conventions.md)中具有 **S** 的 VM 大小通常會支援進階儲存體。 例如，DS 系列、DSv2 系列、GS 系列和 FS 系列的 VM 便支援進階儲存體。 當您選取磁碟大小時，其值會上調為下一個類型。 例如，如果磁碟大小超過 64 GB，但少於 128 GB，則磁碟類型為 P10。 

<br>


[!INCLUDE [disk-storage-premium-ssd-sizes](../../../includes/disk-storage-premium-ssd-sizes.md)]

當您佈建進階儲存體磁碟時，不同於標準儲存體的是，您可獲得該磁碟的容量、IOPS 和輸送量保證。 例如，如果您建立 P50 磁碟，Azure 會為該磁碟佈建 4,095 GB 儲存體容量、7,500 IOPS 和 250 MB/秒的輸送量。 您的應用程式可以使用全部或部分的容量和效能。 進階固態硬碟的設計是為了在 99.9% 的時間內，提供低個位數毫秒延遲以及上表所述的目標 IOPS 和輸送量。

雖然上表指出每個磁碟的最大 IOPS，但可藉由分割多個資料磁碟來達到較高等級的效能。 例如，可以將 64 個資料磁碟連結到 Standard_GS5 VM。 如果上述每個磁碟的大小調整為 P30，就可以達到 80,000 IOPS 的最大值。 如需每部 VM 之最大 IOPS 的詳細資訊，請參閱 [VM 類型和大小](../sizes.md)。

## <a name="launch-azure-cloud-shell"></a>啟動 Azure Cloud Shell

Azure Cloud Shell 是免費的互動式 Shell，可讓您用來執行本文中的步驟。 它具有預先安裝和設定的共用 Azure 工具，可與您的帳戶搭配使用。

若要開啟 Cloud Shell，請選取程式碼區塊右上角的 [試試看]****。 您也可以移至 [https://shell.azure.com/powershell](https://shell.azure.com/bash)，從另一個瀏覽器索引標籤啟動 Cloud Shell。 選取 [複製]  即可複製程式碼區塊，將它貼到 Cloud Shell 中，然後按 enter 鍵加以執行。

## <a name="create-and-attach-disks"></a>建立和連結磁碟

您可以建立資料磁碟並在建立 VM 時連結，或連結至現有的 VM。

### <a name="attach-disk-at-vm-creation"></a>在建立 VM 時連結磁碟

使用 [az group create](/cli/azure/group#az-group-create) 命令來建立資源群組。

```azurecli-interactive
az group create --name myResourceGroupDisk --location eastus
```

使用 [az vm create](/cli/azure/vm#az-vm-create) 命令來建立 VM。 下列範例會建立名為 myVM** 的 VM，新增名為 azureuser** 的使用者帳戶，並產生 SSH 金鑰 (如果沒有這些金鑰的話)。 `--datadisk-sizes-gb` 引數用來指定應該建立一個額外的磁碟並連結至虛擬機器。 若要建立並連結多個磁碟，請使用以空格分隔的磁碟大小值清單。 在下列範例中，會建立具有兩個資料磁碟 (均為 128 GB) 的 VM。 因為磁碟大小是 128 GB，所以這些磁碟都會設為 P10，其可提供每個磁碟最高 500 IOPS。

```azurecli-interactive
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --generate-ssh-keys \
  --data-disk-sizes-gb 128 128
```

### <a name="attach-disk-to-existing-vm"></a>將磁碟連結至現有的 VM

若要建立新的磁碟並將它連結至現有的虛擬機器，請使用 [az vm disk attach](/cli/azure/vm/disk#az-vm-disk-attach) 命令。 下列範例會建立進階磁碟 (大小為 128 GB)，並將它連結至最後一個步驟中建立的 VM。

```azurecli-interactive
az vm disk attach \
    --resource-group myResourceGroupDisk \
    --vm-name myVM \
    --name myDataDisk \
    --size-gb 128 \
    --sku Premium_LRS \
    --new
```

## <a name="prepare-data-disks"></a>準備資料磁碟

將磁碟連結到虛擬機器後，必須將作業系統設定為使用該磁碟。 下列範例示範如何手動設定磁碟。 使用 cloud-init (涵蓋於[稍後的教學課程](./tutorial-automate-vm-deployment.md)中) 也可以將此程序自動化。


建立虛擬機器的 SSH 連線。 以虛擬機器的公用 IP 位址取代範例 IP 位址。

```console
ssh 10.101.10.10
```

使用 `parted` 分割磁碟。

```bash
sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%
```

使用 `mkfs` 命令將檔案系統寫入至磁碟分割。 使用 `partprobe` 讓 OS 知道這項變更。

```bash
sudo mkfs.xfs /dev/sdc1
sudo partprobe /dev/sdc1
```

掛接新磁碟，使其可在作業系統中存取。

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

現在可以透過 `/datadrive` 掛接點存取磁碟，而執行 `df -h` 命令即可驗證此掛接點。

```bash
df -h | grep -i "sd"
```

輸出會顯示掛接在 `/datadrive` 上的新磁碟機。

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        29G  2.0G   27G   7% /
/dev/sda15      105M  3.6M  101M   4% /boot/efi
/dev/sdb1        14G   41M   13G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

為了確保磁碟機會在重新開機之後重新掛接，必須將磁碟機新增至 /etc/fstab** 檔案。 若要這麼做，請使用 `blkid` 公用程式取得磁碟的 UUID。

```bash
sudo -i blkid
```

輸出會顯示磁碟機的 UUID，在此情況下為 `/dev/sdc1`。

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="xfs"
```

> [!NOTE]
> 不當編輯 **/etc/fstab** 檔案會導致系統無法開機。 如果不確定，請參閱散發套件的文件，以取得如何適當編輯此檔案的相關資訊。 在編輯之前，也建議先備份 /etc/fstab 檔案。

在文字編輯器中開啟 `/etc/fstab` 檔案，如下所示：

```bash
sudo nano /etc/fstab
```

將類似下面的一行新增至 */etc/fstab* 檔案，並以您自己的值取代 UUID 值。

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  xfs    defaults,nofail   1  2
```

當您完成檔案的編輯時，請使用 `Ctrl+O` 來寫入檔案，並使用 `Ctrl+X` 來結束編輯器。

現在已設定磁碟，請關閉 SSH 工作階段。

```bash
exit
```

## <a name="take-a-disk-snapshot"></a>擷取磁碟快照集

當您建立磁碟快照集時，Azure 會建立磁碟的唯讀、時間點複本。 進行組態變更之前，Azure VM 快照集可用於快速儲存 VM 的狀態。 發生問題或錯誤時，便可使用快照集來還原 VM。 當 VM 有多個磁碟時，每個磁碟會各自產生快照集。 若要進行應用程式一致備份，請考慮在建立磁碟快照集之前停止 VM。 或者，使用 [Azure 備份服務](../../backup/index.yml)，其可讓您在 VM 執行時執行自動化備份。

### <a name="create-snapshot"></a>建立快照集

建立快照集之前，您需要磁碟的識別碼或名稱。 使用 [az vm show](/cli/azure/vm#az-vm-show) 來拍攝磁碟識別碼。 在此範例中，磁碟識別碼會儲存在變數中，以便用於稍後的步驟。

```azurecli-interactive
osdiskid=$(az vm show \
   -g myResourceGroupDisk \
   -n myVM \
   --query "storageProfile.osDisk.managedDisk.id" \
   -o tsv)
```

現在您已有識別碼，請使用 [az snapshot create](/cli/azure/snapshot#az-snapshot-create) 建立磁碟的快照集。

```azurecli-interactive
az snapshot create \
    --resource-group myResourceGroupDisk \
    --source "$osdiskid" \
    --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>從快照集建立磁碟

此快照集可以接著使用 [az disk create](/cli/azure/disk#az-disk-create) 轉換成磁碟，進而用於重新建立虛擬機器。

```azurecli-interactive
az disk create \
   --resource-group myResourceGroupDisk \
   --name mySnapshotDisk \
   --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>從快照集還原虛擬機器

若要示範虛擬機器復原，請使用 [az vm delete](/cli/azure/vm#az-vm-delete) 刪除現有的虛擬機器。

```azurecli-interactive
az vm delete \
--resource-group myResourceGroupDisk \
--name myVM
```

從快照磁碟建立新的虛擬機器。

```azurecli-interactive
az vm create \
    --resource-group myResourceGroupDisk \
    --name myVM \
    --attach-os-disk mySnapshotDisk \
    --os-type linux
```

### <a name="reattach-data-disk"></a>重新連結資料磁碟

所有資料磁碟都必須重新連結至虛擬機器。

使用 [az disk list](/cli/azure/disk#az-disk-list) 命令尋找資料磁碟名稱。 此範例會將磁碟名稱放入名為 `datadisk` 的變數，該變數使用於下一個步驟。

```azurecli-interactive
datadisk=$(az disk list \
   -g myResourceGroupDisk \
   --query "[?contains(name,'myVM')].[id]" \
   -o tsv)
```

使用 [az vm disk attach](/cli/azure/vm/disk#az-vm-disk-attach) 命令來連結磁碟。

```azurecli-interactive
az vm disk attach \
   –g myResourceGroupDisk \
   --vm-name myVM \
   --name $datadisk
```

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解 VM 磁碟的相關主題，像是：

> [!div class="checklist"]
> * OS 磁碟和暫存磁碟
> * 資料磁碟
> * 標準和進階磁碟
> * 磁碟效能
> * 連結及準備資料磁碟
> * 磁碟快照集

請前進到下一個教學課程，以了解如何自動設定 VM。

> [!div class="nextstepaction"]
> [自動設定 VM](./tutorial-automate-vm-deployment.md)
