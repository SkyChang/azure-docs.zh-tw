---
title: 概觀 - 適用於 MySQL 的 Azure 資料庫單一伺服器
description: 了解適用於 MySQL 的 Azure 資料庫單一伺服器，這是 Microsoft 雲端中以 MySQL Community Edition 為基礎的關聯式資料庫服務。
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc
ms.topic: overview
ms.date: 8/20/2020
ms.openlocfilehash: b33fab7657827733b2c5e7724666a3800686c8d9
ms.sourcegitcommit: f796e1b7b46eb9a9b5c104348a673ad41422ea97
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2020
ms.locfileid: "91564777"
---
# <a name="azure-database-for-mysql-single-server"></a>適用於 MySQL 的 Azure 資料庫單一伺服器

由 MySQL 社群版本提供的[適用於 MySQL 的 Azure 資料庫](overview.md)有二種部署模式：
- 單一伺服器 
- 彈性伺服器 (預覽)

在本文中，我們將提供單一伺服器部署模型之核心概念的概觀和簡介。 如需深入了解彈性伺服器部署模式，請參閱[彈性伺服器概觀](flexible-server/index.yml)。 如需如何決定適合您工作負載部署選項的詳細資訊，請參閱[在 Azure 中選擇正確的 MySQL 伺服器選項](select-right-deployment-type.md)。

## <a name="overview"></a>概觀

單一伺服器是完全受控的資料庫服務，具有自訂資料庫的最低需求。 單一伺服器平台的設計訴求是處理大部分的資料庫管理功能，例如修補、備份、高可用性，並透過最少的使用者組態和控制帶來安全性。 架構已最佳化，可在單一可用性區域上提供 99.99% 的可用性。 同時支援 MySQL 5.6、5.7 和 8.0 社群版本。 本服務已在各個 [Azure 區域](https://azure.microsoft.com/global-infrastructure/services/)中正式推出。 

雲端原生應用程式最適合使用單一伺服器處理自動修補，無需細部控管修補排程和自訂 MySQL 組態設定。 

## <a name="high-availability"></a>高可用性

單一伺服器部署模型已針對內建高可用性最佳化，並提供低成本的選擇彈性。 此模型架構會將計算和儲存體分開。 資料庫引擎會在專屬的計算容器中執行，而資料檔案則位於 Azure 儲存體上。 儲存體會維護三個資料庫檔案本機備援同步複本，確保能維持資料持久性。 

如果伺服器在計畫或未規劃的容錯移轉事件而停止運作，服務會使用下列自動化程序來維護伺服器的高可用性：

1. 佈建新的計算容器
2. 將具有資料檔案的儲存體對應到新的容器 
3. MySQL 資料庫引擎會在新的計算容器上上線
4. 閘道服務可執行透明容錯移轉，以確保不需要變更應用程式端。 
  
一般容錯移轉的時間範圍為 60-120 秒。 單一伺服器服務的雲端原生設計可讓其支援 99.99% 的可用性，降低被動熱待命的成本。

Azure 領先業界的 99.99% 可用性服務等級協定 (SLA)，由 Microsoft 受控資料中心的全球網路提供支援，可協助讓您的應用程式 24 小時全年無休地運作。

:::image type="content" source="media/single-server-overview/1-singleserverarchitecture.png" alt-text="適用於 MySQL 的 Azure 資料庫單一伺服器架構概念圖表"::: 

## <a name="automated-patching"></a>自動修補 

此服務會執行基礎硬體、OS 和資料庫引擎的自動修補。 修補包含安全性和軟體更新。 針對 MySQL 引擎，次要版本升級會自動進行，並包含在修補週期中。 不需要任何使用者動作或組態設定即可進行修補。 修補頻率是由服務根據承載的重要性來進行管理的。 一般而言，服務會遵循持續整合和發行內的每月發行排程。 使用者可以訂閱[計劃性維護通知](concepts-monitoring.md)，以在事件發生前的 72 小時針對即將來臨的維護收到通知。

## <a name="automatic-backups"></a>自動備份

單一伺服器服務會自動建立伺服器備份，並將其儲存在使用者設定的本地備援或異地備援儲存體中。 備份可以在保留期間內將伺服器還原至任何時間點。 預設的備份保留期限是七天。 您可以將保留期設定為最多 35 天。 所有備份皆會使用 AES 256 位元加密進行加密。 如需詳細資訊，請參閱[備份](concepts-backup.md)。

## <a name="adjust-performance-and-scale-within-seconds"></a>在幾秒之內即可調整效能和規模

有三個 SKU 層級提供單一伺服器服務：「基本」、「一般用途」及「記憶體最佳化」。 基本層最適合用於低成本的開發和低並行工作負載。 一般用途和記憶體最佳化較適合需要高並行、縮放和可預測效能的生產工作負載。 您可以在小型資料庫中建置第一個應用程式，一個月只需少許花費，然後調整規模以滿足解決方案的需求。 縮放儲存體比例功能已上線，並支援儲存體自動成長。 動態延展性可讓您的資料庫以透明的方式回應快速變化的資源需求。 您只需就您取用的資源付費。 如需詳細資訊，請參閱 [定價層](concepts-service-tiers.md)。

## <a name="enterprise-grade-security-compliance-and-governance"></a>企業級安全性、合規性和治理

單一伺服器服務針對待用資料儲存體加密是使用符合 FIPS 140-2 的加密模組。 系統會將資料 (包含備份) 和執行查詢時所建立的暫存檔案加密。 該服務使用包含在 Azure 儲存體加密中的 AES 256 位元加密，且金鑰是由系統管理 (預設) 或[由客戶管理](concepts-data-encryption-mysql.md)。 服務預設會以強制執行傳輸層安全性 (SSL/TLS) 的方式來加密資料。 此服務支援 TLS 版本 1.2、1.1 和 1.0，而且能夠強制執行[最低 TLS 版本](concepts-ssl-connection-security.md)。 

此服務可讓您使用[私人連結](concepts-data-access-security-private-link.md)對伺服器進行私人存取，並提供[先進的威脅防護](concepts-data-access-and-security-threat-protection.md)功能。 進階威脅防護會偵測異常活動，指出有不尋常及可能有害的活動試圖存取或惡意探索資料庫。

除了原生驗證之外，單一伺服器服務也支援 [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) 驗證。 Azure AD 驗證是使用 Azure AD 中定義和管理的身分識別連線至 MySQL 伺服器的機制。 透過 Azure AD 驗證，您可以集中管理資料庫使用者的身分識別和其他 Azure 服務，從而簡化和集中控管存取控制。

[稽核記錄](concepts-audit-logs.md)可用來追蹤所有資料庫層級的活動。 

單一伺服器服務與所有業界領先的認證 (例如 FedRAMP、HIPAA、PCI DSS) 相容。 如需 Azure 平台安全性的相關資訊，請造訪 [Azure 信任中心](https://www.microsoft.com/trustcenter/security) \(英文\)。 

如需適用於 MySQL 的 Azure 資料庫安全性功能的詳細資訊，請參閱[安全性概觀](concepts-security.md)。

## <a name="monitoring-and-alerting"></a>監視和警示

單一伺服器服務具備內建的效能監視和警示功能。 所有 Azure 計量都有一分鐘頻率，且每個計量皆提供 30 天的記錄。 您可以在計量上設定警示。 此服務可讓您設定慢速查詢記錄，並提供不同的[查詢存放區](concepts-query-store.md)功能。 查詢存放區可協助您快速找到執行時間最長又最耗資源的查詢，簡化效能疑難排解。 使用這些工具，您可以快速將工作負載最佳化，並設定伺服器以獲得最佳效能。 如需詳細資訊，請參閱[監視](concepts-monitoring.md)。

## <a name="migration"></a>遷移

此服務會執行 MySQL 的社群版本。 這允許完整的應用程式相容性，而且需要的重構成本最少，就能將 MySQL 引擎上開發的現有應用程式遷移至單一伺服器服務。 您可以使用下列其中一個選項移轉單一伺服器：

- **傾印和還原** - 供離線移轉使用，使用者使用 mysqldump/mydumper 等社群工具時，可以承擔一些停機時間、傾印和還原，可最快執行遷移。 參閱[使用傾印和還原遷移](concepts-migrate-dump-restore.md)以取得詳細資料。 
- **Azure 資料庫移轉服務** - 可使用 [Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online)，讓您在最短停機時間的情況下，執行順暢且簡化的單一伺服器移轉。 
- **資料輸入複寫** - 您可以利用使用二進位記錄複寫的資料輸入複寫，在停機時間最短的情況下移轉。 對於想要更充分掌控移轉的專家而言，使用資料輸入複寫能讓停機時間降至最低。 如需詳細資訊，請參閱[資料輸入複寫](concepts-data-in-replication.md)。

## <a name="contacts"></a>連絡人
若您對適用於 MySQL 的 Azure 資料庫有任何疑問或需要建議，請傳送電子郵件給適用於 MySQL 的 Azure 資料庫小組 ([@Ask Azure DB for MySQL](mailto:AskAzureDBforMySQL@service.microsoft.com))。 此電子郵件地址不是技術支援的別名。

此外，請根據您的情況考量下列連絡要點：

- 若要連絡 Azure 支援，[請從 Azure 入口網站提出票證](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)。
- 若要修正您的帳戶問題，請在 Azure 入口網站中提出[支援要求](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)。
- 若要提供意見反應或要求新功能，請透過 [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql) 建立項目。

## <a name="next-steps"></a>後續步驟
您已看過適用於 MySQL 的 Azure 資料庫單一伺服器部署模式的簡介，接下來您可以：

- 建立第一個伺服器。 
  - [使用 Azure 入口網站建立適用於 MySQL 的 Azure 資料庫伺服器](quickstart-create-mysql-server-database-using-azure-portal.md)
  - [使用 Azure CLI 建立適用於 MySQL 的 Azure 資料庫伺服器](quickstart-create-mysql-server-database-using-azure-cli.md)
  - [Azure Database for MySQL 的 Azuer CLI 範例](sample-scripts-azure-cli.md)

- 使用您慣用的語言建置您的第一個應用程式：
  - [Python](./connect-python.md)
  - [Node.JS](./connect-nodejs.md)
  - [Java](./connect-java.md)
  - [Ruby](./connect-ruby.md)
  - [PHP](./connect-php.md)
  - [.NET (C#)](./connect-csharp.md)
  - [Go](./connect-go.md)
  
