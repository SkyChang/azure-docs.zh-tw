---
title: 使用 Azure 入口網站部署適用于 SAP 解決方案的 Azure 監視器
description: 使用 Azure 入口網站部署適用于 SAP 解決方案的 Azure 監視器
author: sameeksha91
ms.author: sakhare
ms.topic: how-to
ms.service: virtual-machines
ms.date: 08/17/2020
ms.reviewer: cynthn
ms.openlocfilehash: 6deb7b535c3876ae8a8e83174b97a75582e82e58
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2020
ms.locfileid: "91996429"
---
# <a name="deploy-azure-monitor-for-sap-solutions-with-azure-portal"></a>使用 Azure 入口網站部署適用于 SAP 解決方案的 Azure 監視器

您可以透過 [Azure 入口網站](https://azure.microsoft.com/features/azure-portal)建立 SAP 解決方案資源的 Azure 監視器。 此方法提供以瀏覽器為基礎的使用者介面，可部署 SAP 解決方案的 Azure 監視器及設定提供者。

## <a name="sign-in-to-azure-portal"></a>登入 Azure 入口網站

在 https://portal.azure.com 登入 Azure 入口網站

## <a name="create-monitoring-resource"></a>建立監視資源

1. 從**Azure Marketplace**選取**適用于 SAP 解決方案的 Azure 監視器**。

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-1.png" alt-text="影像會顯示從 Azure Marketplace 選取 SAP 解決方案供應專案的 Azure 監視器。" lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-1.png":::

2. 在 [ **基本** ] 索引標籤中，提供必要的值。 如果適用，您可以使用現有的 Log Analytics 工作區。

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-2.png" alt-text="影像會顯示從 Azure Marketplace 選取 SAP 解決方案供應專案的 Azure 監視器。" lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-2.png":::

3. 選取虛擬網路時，請確定您要監視的系統可從該 VNET 內連線。 

   > [!IMPORTANT]
   > 選取與 Microsoft 共用資料的 **共用** 可讓我們的支援小組提供其他支援。

## <a name="configure-providers"></a>設定提供者

### <a name="sap-hana-provider"></a>SAP Hana 提供者 

1. 選取 [ **提供者** ] 索引標籤，以新增您想要設定的提供者。 您可以一次新增多個提供者，或在部署監視資源之後將它們加入。 

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-3.png" alt-text="影像會顯示從 Azure Marketplace 選取 SAP 解決方案供應專案的 Azure 監視器。" lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-3.png":::

2. 選取 [ **新增提供者** ]，然後從下拉式清單中選擇 [ **SAP Hana** ]。 

3. 輸入 HANA 伺服器的私人 IP。

4. 輸入您想要使用的資料庫租使用者名稱。 不過，您可以選擇任何租使用者，但我們建議使用 **SYSTEMDB** ，因為它可讓更廣泛的監視區域陣列。 

5. 輸入與 HANA 資料庫相關聯的 SQL 埠號碼。 埠號碼的格式應為 **[3]**  +  **[實例 #]**  +  **[13]** 或 **[3]**  +  **[實例 #]**  +  **[15]**。 例如，30013或30015。 

6. 輸入您想要使用的資料庫使用者名稱。 確定資料庫使用者已指派 **監視** 和 **目錄讀取** 角色。 

7. 完成時，選取 [ **新增提供者**]。 視需要繼續新增其他提供者，或選取 [ **審核 + 建立** ] 以完成部署。

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-4.png" alt-text="影像會顯示從 Azure Marketplace 選取 SAP 解決方案供應專案的 Azure 監視器。" lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-4.png":::

### <a name="high-availability-cluster-pacemaker-provider"></a>高可用性叢集 (Pacemaker) 提供者

1. 從下拉式清單中選取 [ **高可用性叢集 (Pacemaker) ** 。 

   > [!IMPORTANT]
   > 若要設定高可用性叢集 (Pacemaker) 提供者，請確定已在每個節點中安裝 ha_cluster_provider。 如需詳細資訊，請參閱[HA 叢集匯出](https://github.com/ClusterLabs/ha_cluster_exporter#installation)程式

2. 以的形式輸入 Prometheus 端點 http://IP:9664/metrics 。 
 
3. 輸入系統識別碼 (SID) 、主機名稱和叢集名稱。

4. 完成時，選取 [ **新增提供者**]。 視需要繼續新增其他提供者，或選取 [ **審核 + 建立** ] 以完成部署。

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-5.png" alt-text="影像會顯示從 Azure Marketplace 選取 SAP 解決方案供應專案的 Azure 監視器。" lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-5.png":::


### <a name="microsoft-sql-server-provider"></a>Microsoft SQL Server 提供者

1. 在新增 Microsoft SQL Server 提供者之前，您應該在 SQL Server Management Studio 中執行下列腳本，以建立具有設定提供者所需之適當許可權的使用者。

   ```sql
   USE [<Database to monitor>]
   DROP USER [AMS]
   GO
   USE [master]
   DROP USER [AMS]
   DROP LOGIN [AMS]
   GO
   CREATE LOGIN [AMS] WITH PASSWORD=N'<password>', DEFAULT_DATABASE=[<Database to monitor>], DEFAULT_LANGUAGE=[us_english], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF
   CREATE USER AMS FOR LOGIN AMS
   ALTER ROLE [db_datareader] ADD MEMBER [AMS]
   ALTER ROLE [db_denydatawriter] ADD MEMBER [AMS]
   GRANT CONNECT TO AMS
   GRANT VIEW SERVER STATE TO AMS
   GRANT VIEW SERVER STATE TO AMS
   GRANT VIEW ANY DEFINITION TO AMS
   GRANT EXEC ON xp_readerrorlog TO AMS
   GO
   USE [<Database to monitor>]
   CREATE USER [AMS] FOR LOGIN [AMS]
   ALTER ROLE [db_datareader] ADD MEMBER [AMS]
   ALTER ROLE [db_denydatawriter] ADD MEMBER [AMS]
   GO
   ``` 

2. 選取 [ **新增提供者** ]，然後從下拉式清單中選擇 [ **Microsoft SQL Server** ]。 

3. 使用與您 Microsoft SQL Server 相關聯的資訊來填寫欄位。 

4. 完成時，選取 [ **新增提供者**]。 視需要繼續新增其他提供者，或選取 [ **審核 + 建立** ] 以完成部署。

     :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-6.png" alt-text="影像會顯示從 Azure Marketplace 選取 SAP 解決方案供應專案的 Azure 監視器。" lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-6.png":::

## <a name="next-steps"></a>接下來的步驟

深入瞭解 [SAP 解決方案的 Azure 監視器](azure-monitor-overview.md)
