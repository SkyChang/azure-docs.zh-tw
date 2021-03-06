---
title: Azure Integration Runtime IP 位址
description: 瞭解您必須允許輸入流量的 IP 位址，以便適當地設定防火牆來保護資料存放區的網路存取。
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/06/2020
ms.openlocfilehash: 55d8b5ebdfb226247f8a500f36e6df3ae02ea58a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91619039"
---
# <a name="azure-integration-runtime-ip-addresses"></a>Azure Integration Runtime IP 位址

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Integration Runtime 使用的 IP 位址取決於您的 Azure Integration Runtime 所在的區域。 *全部* 相同區域中的 Azure 整合執行時間會使用相同的 IP 位址範圍。

> [!IMPORTANT]  
> 啟用受控虛擬網路的資料流程和 Azure Integration Runtime 不支援使用固定的 IP 範圍。
>
> 您可以針對資料移動、管線和外部活動執行使用這些 IP 範圍。 這些 IP 範圍可用於篩選資料存放區/網路安全性群組 (NSG) /防火牆，以從 Azure Integration runtime 進行輸入存取。 

## <a name="azure-integration-runtime-ip-addresses-specific-regions"></a>Azure Integration Runtime IP 位址：特定區域

允許從您的資源所在的特定 Azure 區域中，針對 Azure Integration runtime 列出的 IP 位址所列出的流量。 您可以從服務標籤 [ip 範圍下載連結](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files)取得服務標記的 ip 範圍清單。 例如，如果 Azure 區域是 **AustraliaEast**，您可以從 DATAFACTORY 取得 IP 範圍清單 **AustraliaEast**。


## <a name="known-issue-with-azure-storage"></a>Azure 儲存體的已知問題

* 連線到 Azure 儲存體帳戶時，IP 網路規則不會影響源自 Azure integration runtime 的要求與儲存體帳戶位於相同區域中的要求。 如需詳細資訊，請 [參閱這篇文章](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range)。 

  相反地，我們建議您 [在連接到 Azure 儲存體時使用信任的服務](https://techcommunity.microsoft.com/t5/azure-data-factory/data-factory-is-now-a-trusted-service-in-azure-storage-and-azure/ba-p/964993)。 

## <a name="next-steps"></a>後續步驟

* [在 Azure Data Factory 中資料移動的安全性考量](data-movement-security-considerations.md)
