---
title: 設定網路存取控制
titleSuffix: Azure SignalR Service
description: 設定您 Azure SignalR Service 的網路存取控制。
services: signalr
author: ArchangelSDY
ms.service: signalr
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: dayshen
ms.openlocfilehash: 72532029b2d9258dba7dea82bb5c5fc8b2673300
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91536217"
---
# <a name="configure-network-access-control"></a>設定網路存取控制

Azure SignalR Service 可讓您根據所用的要求類型和網路子集，保護和控制對服務端點的存取層級。 設定網路規則時，只有透過一組指定網路要求資料的應用程式可以存取您的 Azure SignalR Service。

Azure SignalR Service 具有可透過網際網路存取的公用端點。 您也可以 [為您的 Azure SignalR Service 建立私人端點](howto-private-endpoints.md)。 私人端點會將私人 IP 位址從您的 VNet 指派給 Azure SignalR Service，並透過私人連結保護 VNet 與 Azure SignalR Service 之間的所有流量。 Azure SignalR Service 的網路存取控制提供公用端點和私人端點的存取控制。

（選擇性）您可以選擇允許或拒絕公用端點和每個私人端點的特定類型要求。 例如，您可以封鎖來自公用端點的所有 [伺服器](signalr-concept-internals.md#server-connections) 連線，並確定它們只來自特定的 VNet。

當網路存取控制規則生效時，存取 Azure SignalR Service 的應用程式仍需要適當的要求授權。

## <a name="scenario-a---no-public-traffic"></a>案例 A-沒有公用流量

若要完全拒絕所有公用流量，您應該先將公用網路規則設定為不允許任何要求類型。 然後，您應該設定規則，以授與特定 Vnet 中流量的存取權。 此設定可讓您為應用程式設定安全的網路界限。

## <a name="scenario-b---only-client-connections-from-public-network"></a>案例 B-只有來自公用網路的用戶端連線

在此案例中，您可以將公用網路規則設定為只允許來自公用網路的 [用戶端](signalr-concept-internals.md#client-connections) 連線。 然後，您可以設定私人網路規則，以允許來自特定 VNet 的其他類型要求。 此設定會從公用網路隱藏您的應用程式伺服器，並在您的應用程式伺服器和 Azure SignalR Service 之間建立安全的連線。

## <a name="managing-network-access-control"></a>管理網路存取控制

您可以透過 Azure 入口網站管理 Azure SignalR Service 的網路存取控制。

### <a name="azure-portal"></a>Azure 入口網站

1. 移至您想要保護的 Azure SignalR Service。

1. 按一下名為 [ **網路存取控制**] 的 [設定] 功能表。

    ![入口網站上的網路 ACL](media/howto-network-access-control/portal.png)

1. 若要編輯預設動作，請切換 [ **允許/拒絕** ] 按鈕。

    > [!TIP]
    > 預設動作是在沒有任何 ACL 規則相符時所採取的動作。 例如，如果預設動作是 [ **拒絕**]，則未明確核准的要求類型將會遭到拒絕。

1. 若要編輯公用網路規則，請選取 [ **公用網路**] 下允許的要求類型。

    ![在入口網站上編輯公用網路 ACL ](media/howto-network-access-control/portal-public-network.png)

1. 若要編輯私人端點網路規則，請在 [ **私人端點連接**] 底下的每個資料列中，選取允許的要求類型。

    ![在入口網站上編輯私人端點 ACL ](media/howto-network-access-control/portal-private-endpoint.png)

1. 按一下 [儲存] 套用變更。

## <a name="next-steps"></a>後續步驟

深入了解 [Azure Private Link](/azure/private-link/private-link-overview)。
