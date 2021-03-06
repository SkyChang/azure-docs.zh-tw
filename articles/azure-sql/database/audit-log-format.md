---
title: SQL Database audit 記錄檔格式
description: 瞭解 Azure SQL Database 審核記錄的結構。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: reference
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.custom: sqldbrb=1
ms.date: 06/03/2020
ms.openlocfilehash: 49e885862a49bb3bbea718566ee5389e6b08e860
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91444604"
---
# <a name="sql-database-audit-log-format"></a>SQL Database audit 記錄檔格式

[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

[Azure SQL Database 審核](auditing-overview.md) 會追蹤資料庫事件，並將它們寫入至您 Azure 儲存體帳戶中的 audit 記錄，或傳送至事件中樞或 log Analytics 以進行下游處理和分析。

## <a name="naming-conventions"></a>命名規範

### <a name="blob-audit"></a>Blob 審核

儲存在 Azure Blob 儲存體中的 Audit 記錄會儲存在 Azure 儲存體帳戶中名為的容器 `sqldbauditlogs` 。 容器內的目錄階層是表單 `<ServerName>/<DatabaseName>/<AuditName>/<Date>/` 。 Blob 檔案名格式為 `<CreationTime>_<FileNumberInSession>.xel` ，其中 `CreationTime` 是 UTC `hh_mm_ss_ms` 格式，而且 `FileNumberInSession` 是一個執行中的索引，以因應會話記錄橫跨多個 Blob 檔案的情況。

例如，以下的資料庫 `Database1` `Server1` 可能是有效的路徑：

`Server1/Database1/SqlDbAuditing_ServerAudit_NoRetention/2019-02-03/12_23_30_794_0.xel`

[唯讀複本](read-scale-out.md) 審核記錄會儲存在相同的容器中。 容器內的目錄階層是表單 `<ServerName>/<DatabaseName>/<AuditName>/<Date>/RO/` 。 Blob 檔案名會共用相同的格式。 唯讀複本的審核記錄會儲存在相同的容器中。


### <a name="event-hub"></a>事件中樞

Audit 事件會寫入至在審核設定期間所定義的命名空間和事件中樞，並在 [Apache Avro](https://avro.apache.org/) 事件主體中捕捉，並使用 utf-8 編碼的 JSON 格式來儲存。 若要讀取稽核記錄，您可以使用 [Avro Tools](../../event-hubs/event-hubs-capture-overview.md#use-avro-tools) 或類似工具來處理這種格式。

### <a name="log-analytics"></a>Log Analytics

Audit 事件會寫入至在審核設定期間定義的 Log Analytics 工作區，並寫入至 `AzureDiagnostics` 具有類別目錄的資料表 `SQLSecurityAuditEvents` 。 如需 Log Analytics 搜尋語言和命令的其他實用資訊，請參閱 [Log Analytics 搜尋參考](../../azure-monitor/log-query/log-query-overview.md)。

## <a name="audit-log-fields"></a><a id="subheading-1"></a>審核記錄欄位

|  (blob) 名稱 |  (事件中樞/Log Analytics) 的名稱 | 描述 | Blob 類型 | 事件中樞/Log Analytics 類型 |
|-------------|---------------------------------|-------------|-----------|-------------------------------|
| action_id | action_id_s | 動作的識別碼 | varchar(4) | 字串 |
| action_name | action_name_s | 動作的名稱 | N/A | 字串 |
| additional_information | additional_information_s | 有關事件的任何其他資訊，儲存為 XML | nvarchar(4000) | 字串 |
| affected_rows | affected_rows_d | 受查詢影響的資料列數目 | BIGINT | int |
| application_name | application_name_s| 用戶端應用程式的名稱 | nvarchar(128) | 字串 |
| audit_schema_version | audit_schema_version_d | 一律為1 | int | int |
| class_type | class_type_s | 進行審核的可審核實體類型 | varchar(2) | 字串 |
| class_type_desc | class_type_description_s | 進行審核之可審核實體的描述 | N/A | 字串 |
| client_ip | client_ip_s | 用戶端應用程式的來源 IP | nvarchar(128) | 字串 |
| connection_id | N/A | 伺服器中的連接識別碼 | GUID | N/A |
| data_sensitivity_information | data_sensitivity_information_s | 經過審核查詢所傳回的資訊類型和敏感度標籤，是根據資料庫中的分類資料行。 深入瞭解 [Azure SQL Database 資料探索和分類](data-discovery-and-classification-overview.md) | nvarchar(4000) | 字串 |
| database_name | database_name_s | 發生動作的資料庫內容 | sysname | 字串 |
| database_principal_id | database_principal_id_d | 動作執行所在之資料庫使用者內容的識別碼 | int | int |
| database_principal_name | database_principal_name_s | 執行動作的資料庫使用者內容名稱 | sysname | 字串 |
| duration_milliseconds | duration_milliseconds_d | 查詢執行持續期間（毫秒） | BIGINT | int |
| event_time | event_time_t | 引發可審核動作的日期和時間 | datetime2 | Datetime |
| host_name | N/A | 用戶端主機名稱 | 字串 | N/A |
| is_column_permission | is_column_permission_s | 指出這是否為資料行層級權限的旗標。 1 = true，0 = false | bit | 字串 |
| N/A | is_server_level_audit_s | 指出此審核是否在伺服器層級的旗標 | N/A | 字串 |
| object_ id | object_id_d | 稽核發生所在之實體的識別碼。 這包括：伺服器物件、資料庫、資料庫物件和架構物件。 如果實體是伺服器本身，或如果未在物件層級執行審核，則為0。 | int | int |
| object_name | object_name_s | 稽核發生所在之實體的名稱。 這包括：伺服器物件、資料庫、資料庫物件和架構物件。 如果實體是伺服器本身，或如果未在物件層級執行審核，則為0。 | sysname | 字串 |
| permission_bitmask | permission_bitmask_s | 當適用時，顯示已授與、拒絕或撤銷的權限 | varbinary(16) | 字串 |
| response_rows | response_rows_d | 結果集中傳回的資料列數目 | BIGINT | int |
| schema_name | schema_name_s | 動作發生所在的結構描述環境。 如果是在架構之外發生的審核，則為 Null | sysname | 字串 |
| N/A | securable_class_type_s | 對應至正在進行審核之 class_type 的安全物件 | N/A | 字串 |
| sequence_group_id | sequence_group_id_g | 唯一識別碼 | varbinary | GUID |
| sequence_number | sequence_number_d | 追蹤單一 audit 記錄內的記錄順序，該記錄太大而無法納入進行審核的寫入緩衝區 | int | int |
| server_instance_name | server_instance_name_s | 發生審核之伺服器實例的名稱 | sysname | 字串 |
| server_principal_id | server_principal_id_d | 執行動作之登入內容的識別碼 | int | int |
| server_principal_name | server_principal_name_s | 目前的登入 | sysname | 字串 |
| server_principal_sid | server_principal_sid_s | 目前的登入 SID | varbinary | 字串 |
| session_id | session_id_d | 發生事件之會話的識別碼 | SMALLINT | int |
| session_server_principal_name | session_server_principal_name_s | 會話的伺服器主體 | sysname | 字串 |
| 陳述式 | statement_s | 執行 (的 t-sql 語句（如果有的話）)  | nvarchar(4000) | 字串 |
| 成功 | succeeded_s | 指示觸發此事件的動作是否成功。 針對登入和批次以外的事件，這只會報告許可權檢查成功或失敗，而不是作業。 1 = 成功，0 = 失敗 | bit | 字串 |
| target_database_principal_id | target_database_principal_id_d | GRANT/DENY/REVOKE 作業執行所在的資料庫主體。 0（如果不適用） | int | int |
| target_database_principal_name | target_database_principal_name_s | 動作的目標使用者。 Null （如果不適用） | 字串 | 字串 |
| target_server_principal_id | target_server_principal_id_d | GRANT/DENY/REVOKE 作業執行所在的伺服器主體。 如果不適用則傳回0 | int | int |
| target_server_principal_name | target_server_principal_name_s | 動作的目標登入。 Null （如果不適用） | sysname | 字串 |
| target_server_principal_sid | target_server_principal_sid_s | 目標登入的 SID。 Null （如果不適用） | varbinary | 字串 |
| transaction_id | transaction_id_d | 只有 (從2016開始 SQL Server 的) -0 代表 Azure SQL Database | BIGINT | int |
| user_defined_event_id | user_defined_event_id_d | 以引數形式傳遞給 sp_audit_write 的使用者定義事件識別碼。 系統事件的 Null (預設值) ，而使用者定義的事件則為非零。 如需詳細資訊，請參閱 [sp_audit_write (transact-sql) ](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | SMALLINT | int |
| user_defined_information | user_defined_information_s | 以引數形式傳遞給 sp_audit_write 的使用者定義資訊。 系統事件的 Null (預設值) ，而使用者定義的事件則為非零。 如需詳細資訊，請參閱 [sp_audit_write (transact-sql) ](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | nvarchar(4000) | 字串 |

## <a name="next-steps"></a>後續步驟

深入瞭解 [Azure SQL Database 的審核](auditing-overview.md)。
