---
title: Azure Analysis Services 備份與還原 | Microsoft Docs
description: 本文說明如何從 Azure Analysis Services 資料庫備份和還原模型中繼資料和資料。
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: owend
ms.reviewer: minewiskan
ms.custom: references_regions
ms.openlocfilehash: af1850f77c1d13c761bfc2a143074b5067b349b4
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2020
ms.locfileid: "92014047"
---
# <a name="analysis-services-database-backup-and-restore"></a>Analysis Services 資料庫備份和還原

在 Azure Analysis Services 中備份表格式模型資料庫與內部部署的 Analysis Services 情況非常類似。 主要的差異在於備份檔案的儲存位置。 備份檔案必須儲存至 [Azure 儲存體帳戶](../storage/common/storage-account-create.md)中的容器。 您可以使用您已經有的儲存體帳戶和容器，或是在為您的伺服器設定儲存體設定時再建立帳戶和容器。

> [!NOTE]
> 建立儲存體帳戶會導致產生新的可計費服務。 若要深入了解，請參閱 [Azure 儲存體定價](https://azure.microsoft.com/pricing/details/storage/blobs/)。
> 
> 

> [!NOTE]
> 如果儲存體帳戶位於不同的區域，請設定儲存體帳戶防火牆設定，以允許從 **選取的網路**進行存取。 在 [防火牆 **位址範圍**] 中，指定 Analysis Services 伺服器所在區域的 IP 位址範圍。 您可以設定儲存體帳戶防火牆設定以允許來自所有網路的存取，不過最好是選擇 [選取的網路]，並指定 [IP 位址範圍]。 若要深入瞭解，請參閱 [網路連接常見問題](analysis-services-network-faq.md#backup-and-restore)。

備份會以 .abf 副檔名儲存。 針對記憶體內表格式模型，會同時儲存模型資料和中繼資料。 針對 DirectQuery 表格式模型，則只會儲存模型中繼資料。 視您選擇的選項而定，可以將備份壓縮和加密。


## <a name="configure-storage-settings"></a>設定儲存體設定
進行備份之前，您必須為伺服器設定儲存體設定。


### <a name="to-configure-storage-settings"></a>設定儲存體設定
1.  在 Azure 入口網站 > [設定]**** 中，按一下 [備份]****。

    ![[設定] 中的 [備份]](./media/analysis-services-backup/aas-backup-backups.png)

2.  按一下 [已啟用]****，然後按一下 [儲存體設定]****。

    ![啟用](./media/analysis-services-backup/aas-backup-enable.png)

3. 選取您的儲存體帳戶，或建立新帳戶。

4. 選取容器，或建立新容器。

    ![選取容器](./media/analysis-services-backup/aas-backup-container.png)

5. 儲存您的備份設定。

    ![儲存備份設定](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Backup

### <a name="to-backup-by-using-ssms"></a>使用 SSMS 來進行備份

1. 在 SSMS 中，於資料庫上按一下滑鼠右鍵 > [備份]****。

2. 在 [**備份資料庫**  >  **備份檔案**] 中，按一下 **[流覽]**。

3. 在 [另存新檔]**** 對話方塊中，確認資料夾路徑，然後輸入備份檔案的名稱。 

4. 在 [備份資料庫]**** 對話方塊中，選取選項。

    **允許檔案覆寫** - 若要覆寫同名的備份檔案，請選取此選項。 如果未選取此選項，您要儲存的檔案就不能與相同位置中已經存在的檔案同名。

    **套用壓縮** - 若要壓縮備份檔案，請選取此選項。 壓縮過的備份檔案可節省磁碟空間，但需要使用稍微多一點的 CPU 資源。 

    **加密備份檔案** - 若要將備份檔案加密，請選取此選項。 此選項需要有使用者提供的密碼來保護備份檔案。 此密碼可防止以還原作業以外的任何其他方式讀取備份資料。 如果您選擇將備份加密，請將密碼儲存在安全的位置。

5. 按一下 [確定]**** 以建立並儲存備份檔案。


### <a name="powershell"></a>PowerShell
使用 [Backup-ASDatabase](/powershell/module/sqlserver/backup-asdatabase) Cmdlet。

## <a name="restore"></a>還原
在還原時，您的備份檔案必須位於您為伺服器所設定的儲存體帳戶中。 如果您需要將備份檔案從內部部署位置移至儲存體帳戶，請使用 [Microsoft Azure 儲存體總管](../vs-azure-tools-storage-manage-with-storage-explorer.md)或 [AzCopy](../storage/common/storage-use-azcopy-v10.md) 命令列公用程式。 



> [!NOTE]
> 如果您要從內部部署伺服器進行還原，則必須先從該模型的角色中移除所有網域使用者，再將他們新增回角色中成為 Azure Active Directory 使用者。
> 
> 

### <a name="to-restore-by-using-ssms"></a>使用 SSMS 來進行還原

1. 在 SSMS 中，於資料庫上按一下滑鼠右鍵 > [還原]****。

2. 在 [備份資料庫]**** 對話方塊的 [備份檔案]**** 中，按一下 [瀏覽]****。

3. 在 [尋找資料庫檔案]**** 對話方塊中，選取您想要還原的檔案。

4. 在 [還原資料庫]**** 中，選取資料庫。

5. 指定選項。 安全性選項必須與您備份時所使用的備份選項相符。


### <a name="powershell"></a>PowerShell

使用 [Restore-ASDatabase](/powershell/module/sqlserver/restore-asdatabase) Cmdlet。


## <a name="related-information"></a>相關資訊

[Azure 儲存體帳戶](../storage/common/storage-account-create.md)  
[高可用性](analysis-services-bcdr.md)      
[Analysis Services 網路連線常見問題](analysis-services-network-faq.md)