---
title: 儲存體 FSLogix 設定檔容器 Windows 虛擬桌面-Azure
description: 將您的 Windows 虛擬桌面 FSLogix 設定檔儲存在 Azure 儲存體上的選項。
author: Heidilohr
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 0b1a5e36232e74caa34037efbbb0da0c39051998
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89568688"
---
# <a name="storage-options-for-fslogix-profile-containers-in-windows-virtual-desktop"></a>在 Windows 虛擬桌面中 FSLogix 設定檔容器的儲存體選項

Azure 提供多個儲存體解決方案，可供您用來儲存 FSLogix 設定檔容器。 本文將比較 Azure 為 Windows 虛擬桌面 FSLogix 使用者設定檔容器提供的儲存體解決方案。 建議您將 FSLogix 設定檔容器儲存在適用于大部分客戶的 Azure 檔案儲存體上。

Windows 虛擬桌面提供 FSLogix 設定檔容器作為建議的使用者設定檔解決方案。 FSLogix 可用來漫遊遠端運算環境中的設定檔，例如 Windows 虛擬桌面。 在登入時，此容器會使用原生支援的虛擬硬碟 (VHD) 和 Hyper-v 虛擬硬碟 (VHDX) ，以動態方式連接到計算環境。 使用者設定檔能夠立即使用，且會與原生使用者設定檔一樣完全出現在系統中。

下表比較適用于 Windows 虛擬桌面 FSLogix 設定檔容器使用者設定檔的存放裝置解決方案 Azure 儲存體優惠。

## <a name="azure-platform-details"></a>Azure 平臺詳細資料

|特性|Azure 檔案|Azure NetApp Files|儲存空間直接存取|
|--------|-----------|------------------|---------------------|
|使用案例|一般用途|從 NetApp 內部部署的 Ultra 效能或遷移|跨平台|
|平臺服務|是，Azure-原生解決方案|是，Azure-原生解決方案|否，自我管理|
|區域可用性|所有區域|[選取區域](https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all)|所有區域|
|備援性|本地區域冗余/區域-冗余/地理位置多餘|本機多餘|本地區域冗余/區域-冗余/地理位置多餘|
|層級和效能|標準<br>Premium<br>每個共用最多10萬 IOPS，每個共用 5 GBps，大約3毫秒延遲|標準<br>Premium<br>Ultra<br>最高 320k (16K) IOPS，每個磁片區 4.5 GBps，大約1毫秒延遲|標準 HDD：最高 500 IOPS 的每一磁片限制<br>標準 SSD：最高 4k IOP 的每一磁片限制<br>進階 SSD：每個磁片的最高 20k IOPS 限制<br>建議儲存空間直接存取的 Premium 磁片|
|Capacity|每個共用 100 TiB|每個磁片區 100 TiB，每個訂用帳戶最多 12.5 PiB|每個磁片最大 32 TiB|
|必要的基礎結構|共用大小下限 1 GiB|容量集區 4 TiB、最小磁片區大小 100 GiB|Azure IaaS 上的兩部 Vm (+ 雲端見證) 或至少三個 Vm，但不含磁片的費用|
|通訊協定|SMB 2.1/3。 和 REST|NFSv3、Nfsv4.1 4.1 (preview) 、SMB 3.x/2。x|NFSv3、Nfsv4.1 4.1、SMB 3。1|

## <a name="azure-management-details"></a>Azure 管理詳細資料

|特性|Azure 檔案|Azure NetApp Files|儲存空間直接存取|
|--------|-----------|------------------|---------------------|
|存取|雲端、內部部署和混合式 (Azure 檔案同步) |雲端、內部部署 (via ExpressRoute) |雲端、內部部署|
|Backup|Azure 備份快照集整合|Azure NetApp Files 快照集|Azure 備份快照集整合|
|安全性與合規性|[所有 Azure 支援的憑證](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|ISO 已完成|[所有 Azure 支援的憑證](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|
|Azure Active Directory 整合|[原生 Active Directory 和 Azure Active Directory Domain Services](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-overview)|[Azure Active Directory Domain Services 和原生 Active Directory](../azure-netapp-files/azure-netapp-files-faqs.md#does-azure-netapp-files-support-azure-active-directory)|原生 Active Directory 或僅 Azure Active Directory Domain Services 支援|

選擇您的儲存方法之後，請參閱 [Windows 虛擬桌面定價](https://azure.microsoft.com/pricing/details/virtual-desktop/) 以取得定價方案的相關資訊。

## <a name="next-steps"></a>後續步驟

若要深入瞭解 FSLogix 設定檔容器、使用者設定檔磁片和其他使用者設定檔技術，請參閱 [FSLogix 設定檔容器和 Azure 檔案](fslogix-containers-azure-files.md)儲存體中的表格。

如果您已準備好建立自己的 FSLogix 設定檔容器，請從下列其中一個教學課程開始：

- [開始在 Windows 虛擬桌面中 Azure 檔案儲存體的 FSLogix 設定檔容器](create-file-share.md)
- [使用 Azure NetApp files 建立主機集區的 FSLogix 設定檔容器](create-fslogix-profile-container.md)
- 當您使用 FSLogix 設定檔容器（而非使用者設定檔磁片）時，在 [Azure 中部署 UPD 儲存體的雙節點儲存空間直接存取向外延展檔案伺服器](/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment/) 中的指示也適用

您也可以從一開始著手，並在 [Windows 虛擬桌面的 [建立租](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md)使用者] 設定您自己的 Windows 虛擬桌面解決方案。
