---
title: 備份中心-常見問題
description: 本文將回答有關備份中心的常見問題
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: b10a9e73e65cf12c43ce28b429a8f12e0b960a76
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "90994801"
---
# <a name="backup-center---frequently-asked-questions"></a>備份中心-常見問題

## <a name="management"></a>管理性

### <a name="can-backup-center-be-used-across-tenants"></a>可以跨租使用者使用備份中心嗎？

是的，如果您使用 [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/overview) ，並對不同租使用者的訂用帳戶委派存取權，您可以使用 [備份中心] 作為單一管理平臺，以管理跨租使用者的備份。

### <a name="can-backup-center-be-used-to-manage-both-recovery-services-vaults-and-backup-vaults"></a>備份中心可以用來管理復原服務保存庫和備份保存庫嗎？

是，備份中心可以同時管理復原 [服務保存庫](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) 和 [備份](backup-vault-overview.md)保存庫。

### <a name="is-there-a-delay-before-data-surfaces-in-backup-center"></a>備份中心的資料面之前是否有延遲？

備份中心的目標是提供即時資訊。 實體在個別保存庫畫面上顯示的時間可能會有幾秒鐘的時間，以及在備份中心內顯示相同實體的時間。

## <a name="configuration"></a>組態

### <a name="do-i-need-to-configure-anything-to-see-data-in-backup-center"></a>我是否需要在備份中心內設定任何專案來查看資料？

否。 備份中心已準備就緒。 不過，若要在 [備份中心] 下查看 [備份報告](https://docs.microsoft.com/azure/backup/configure-reports) ，您必須設定保存庫的報告。

### <a name="do-i-need-to-have-any-special-permissions-to-use-backup-center"></a>我是否需要有任何特殊許可權才能使用備份中心？

這類的備份中心並不需要任何新的許可權。 只要您針對所管理的資源擁有正確的 RBAC 存取層級，您就可以使用這些資源的「備份中心」。 例如，若要查看備份的相關資訊，您需要有您保存庫的 **讀取** 者存取權。 若要設定備份和執行其他備份相關的動作，您將需要「 **備份參與者** 」或「 **備份操作員** 」角色。 深入瞭解 [Azure 備份的 RBAC 角色](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault)。

## <a name="pricing"></a>定價

### <a name="do-i-need-to-pay-anything-extra-to-use-backup-explorer"></a>我是否需要支付任何額外的費用，以使用 Backup Explorer？

目前，除了您的備份成本之外，還沒有額外的成本 (使用備份中心) 。 但是，如果您在 [備份中心] 下使用 [備份報告](https://docs.microsoft.com/azure/backup/configure-reports) ，則需要使用備份報告的 Azure 監視器記錄的 [相關成本](https://azure.microsoft.com/pricing/details/monitor/) 。

## <a name="next-steps"></a>後續步驟

閱讀其他常見問題集：

* [復原服務保存庫的相關常見問題](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq)
* [關於 Azure VM 備份的常見問題](https://docs.microsoft.com/azure/backup/backup-azure-vm-backup-faq)
