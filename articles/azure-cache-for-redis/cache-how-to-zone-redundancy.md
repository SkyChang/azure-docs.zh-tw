---
title: '針對 Azure Cache for Redis (預覽版啟用區域冗余) '
description: 瞭解如何為您的進階層 Azure Cache for Redis 實例設定區域冗余
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 33c346fa2e4572799ad6341bd5115cdd6e3b9ec9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91569982"
---
# <a name="enable-zone-redundancy-for-azure-cache-for-redis-preview"></a>針對 Azure Cache for Redis (預覽版啟用區域冗余) 
在本文中，您將瞭解如何使用 Azure 入口網站設定區域冗余的 Azure 快取實例。

Azure Cache for Redis Standard 和 Premium 層提供內建的虛擬機器，方法是將每個快取裝載在兩部專用的虛擬機器上， (Vm) 。 雖然這些 Vm 位於不同的 [Azure 容錯和更新網域](/azure/virtual-machines/windows/manage-availability) ，且具有高可用性，但它們很容易發生資料中心層級的失敗。 Azure Cache for Redis 也支援其進階層中的區域冗余。 區域冗余快取會在分散到多個 [可用性區域](/azure/virtual-machines/windows/manage-availability#use-availability-zones-to-protect-from-datacenter-level-failures)的 vm 上執行。 它提供更高的復原能力和可用性。

> [!IMPORTANT]
> 此預覽版是在沒有服務等級協定的情況下提供，不建議用於生產工作負載。 如需詳細資訊，請參閱 [Microsoft Azure 預覽的補充使用條款。](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 
> 

## <a name="prerequisites"></a>必要條件
* Azure 訂用帳戶- [免費建立一個](https://azure.microsoft.com/free/)訂用帳戶

> [!NOTE]
> 這項功能目前為預覽狀態，如果您有興趣， [請洽詢我們](mailto:azurecache@microsoft.com) 。
>

## <a name="create-a-cache"></a>建立快取
若要建立快取，請遵循下列步驟：

1. 登入 [Azure 入口網站](https://portal.azure.com)，然後選取 [建立資源]****。
  
1. 在 [新增]  頁面上選取 [資料庫]  ，然後選取 [Azure Cache for Redis]  。

    :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="選取 Azure Cache for Redis。":::
   
1. 在 [ **基本** ] 頁面上，設定新快取的設定。
   
    | 設定      | 建議的值  | 描述 |
    | ------------ |  ------- | -------------------------------------------------- |
    | **訂用帳戶** | 選取您的訂用帳戶。 | 這個新的 Azure Cache for Redis 執行個體建立所在的訂用帳戶。 | 
    | **資源群組** | 選取資源群組，或選取 [ **建立新** 的]，並輸入新的資源組名。 | 用來建立快取和其他資源的資源群組名稱。 將所有的應用程式資源放在一個資源群組中，您將可輕鬆地一併管理或刪除這些資源。 | 
    | **DNS 名稱** | 輸入全域唯一名稱。 | 快取名稱必須是 1 到 63 個字元的字串，且只能包含數字、字母或連字號。 名稱的開頭和結尾必須是數字或字母，且不可包含連續的連字號。 您的快取執行個體*主機名稱*將是 *\<DNS name>.redis.cache.windows.net*。 | 
    | **位置** | 選取位置。 | 選取其他將使用快取的服務附近的[區域](https://azure.microsoft.com/regions/)。 |
    | **快取類型** | 選取 [Premium 層](https://azure.microsoft.com/pricing/details/cache/) 快取。 |  快取的可用大小、效能和功能取決於定價層。 如需詳細資訊，請參閱 [Azure Cache for Redis 概觀](cache-overview.md)。 |
   
1. 在 [ **Advanced** ] 頁面上，選擇 [ **複本計數**]。
   
    :::image type="content" source="media/cache-how-to-multi-replicas/create-multi-replicas.png" alt-text="選取 Azure Cache for Redis。":::

1. 選取 [ **可用性區域**]。 
   
    :::image type="content" source="media/cache-how-to-zone-redundancy/create-zones.png" alt-text="選取 Azure Cache for Redis。":::

1. 將其他選項保留為預設設定。 

    > [!NOTE]
    > 區域冗余支援目前僅適用于非叢集和非異地複寫的快取。 此外，它不支援私用連結、調整、資料持續性或匯入/匯出。
    >

1. 按一下 [建立]。 
   
    建立快取需要一些時間。 您可以在 Azure Cache for Redis 的 [概觀] 頁面上監視進度。 當 [狀態] 顯示為 [執行中] 時，表示快取已可供使用。
   
    > [!NOTE]
    > 建立快取之後，就無法變更可用性區域。
    >

## <a name="next-steps"></a>後續步驟
深入瞭解 Azure Cache for Redis 功能。

> [!div class="nextstepaction"]
> [Azure Cache for Redis Premium 服務層級](cache-overview.md#service-tiers)
