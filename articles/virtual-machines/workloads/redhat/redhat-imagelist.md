---
title: Azure 中可用的 Red Hat Enterprise Linux 映射
description: 了解 Microsoft Azure 中的 Red Hat Enterprise Linux 映像
author: asinn826
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/16/2020
ms.author: alsin
ms.reviewer: cynthn
ms.openlocfilehash: 628e9098eefa311f3ee5603b9eaf633d67d60c5f
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2020
ms.locfileid: "91994347"
---
# <a name="red-hat-enterprise-linux-rhel-images-available-in-azure"></a>Red Hat Enterprise Linux Azure 中提供的 (RHEL) 映射
Azure 提供各種不同使用案例的 RHEL 映射。

> [!NOTE]
> 所有 RHEL 映射都可在 Azure 公用和 Azure Government 雲端中使用。 它們無法在 Azure 中國雲端中使用。

## <a name="list-of-rhel-images"></a>RHEL 映射清單
這是 Azure 中可用的 RHEL 映射清單。 除非另有說明，否則所有映射都是 LVM 分割並附加至一般 RHEL 存放庫， (不 EUS，而不是 E4S) 。 以下是目前可供一般使用的映射：

> [!NOTE]
> 不會再產生原始影像，而是要改用 LVM 分割的影像。 LVM 提供許多優於舊版原始 (非 LVM) 資料分割配置的優點，包括大幅提高彈性的分割區大小選項。

供應項目| SKU | 資料分割 | 佈建 | 附註
:----|:----|:-------------|:-------------|:-----
RHEL          | 6.7      | RAW    | Linux 代理程式 |
|             | 6.8      | RAW    | Linux 代理程式 |
|             | 6.9      | RAW    | Linux 代理程式 |
|             | 6.10     | RAW    | Linux 代理程式 |
|             | 7-RAW    | RAW    | Linux 代理程式 | RHEL 7.x 映射系列。 <br> 預設會附加至一般存放庫， (不會 EUS) 。
|             | 7-LVM    | LVM    | Linux 代理程式 | RHEL 7.x 映射系列。 <br> 預設會附加至一般存放庫， (不會 EUS) 。 如果您要尋找標準 RHEL 映射以進行部署，請使用這組映射及/或其第2代對應。
|             | 7lvm-gen2| LVM    | Linux 代理程式 | 第2代、RHEL 7.x 映射系列。 <br> 預設會附加至一般存放庫， (不會 EUS) 。 如果您要尋找要部署的標準 RHEL 映射，請使用這組映射及/或其第1代。
|             | 7-RAW-CI | RAW-CI | Cloud-init  | RHEL 7.x 映射系列。 <br> 預設會附加至一般存放庫， (不會 EUS) 。
|             | 7.2      | RAW    | Linux 代理程式 |
|             | 7.3      | RAW    | Linux 代理程式 |
|             | 7.4      | RAW    | Linux 代理程式 | 從2019年4月起，預設已附加至 EUS 存放庫。
|             | 74-gen2  | RAW    | Linux 代理程式 | 預設會附加至 EUS 存放庫。
|             | 7.5      | RAW    | Linux 代理程式 | 從2019年6月起，預設已附加至 EUS 存放庫。
|             | 75-gen2  | RAW    | Linux 代理程式 | 預設會附加至 EUS 存放庫。
|             | 7.6      | RAW    | Linux 代理程式 | 預設會附加至 EUS 存放庫，從2019到5月。
|             | 76-gen2  | RAW    | Linux 代理程式 | 預設會附加至 EUS 存放庫。
|             | 7.7      | LVM    | Linux 代理程式 | 預設會附加至 EUS 存放庫。
|             | 77-gen2  | LVM    | Linux 代理程式 | 預設會附加至 EUS 存放庫。
|             | 7.8      | LVM    | Linux 代理程式 | 連結至一般存放庫 (EUS 無法用於 RHEL 7.8) 
|             | 78-gen2  | LVM    | Linux 代理程式 | 連結至一般存放庫 (EUS 無法用於 RHEL 7.8) 
|             | 8-LVM    | LVM    | Linux 代理程式 | RHEL 8.x 系列映射。 附加至一般存放庫。
|             | 8-lvm-gen2| LVM    | Linux 代理程式 | Hyper-v 第2代-RHEL 8.x 系列映射。 附加至一般存放庫。
|             | 8        | LVM    | Linux 代理程式 | RHEL 8.0 映射。
|             | 8-gen2   | LVM    | Linux 代理程式 | Hyper-v 第2代-RHEL 8.0 映射。
|             | 8.1      | LVM    | Linux 代理程式 | RHEL 8.2 映射。 目前已附加至一般存放庫。
|             | 81gen2   | LVM    | Linux 代理程式 | Hyper-v 第2代-RHEL 8.1 映射。 目前已附加至一般存放庫。
|             | 8.1-ci   | LVM    | Linux 代理程式 | 使用雲端初始作為布建代理程式的 RHEL 8.1 映射。 目前已附加至一般存放庫。
|             | 81-ci-gen2| LVM    | Linux 代理程式 | Hyper-v 第2代-使用雲端初始作為布建代理程式的 RHEL 8.1 映射。 目前已附加至一般存放庫。
|             | 8.2      | LVM    | Linux 代理程式 | RHEL 8.2 映射。 目前已附加至一般存放庫。
|             | 82gen2   | LVM    | Linux 代理程式 | Hyper-v 第2代-RHEL 8.1 映射。 目前已附加至一般存放庫。
RHEL-SAP      | 7.4      | LVM    | Linux 代理程式 | 適用于 SAP Hana 及商務應用程式的 RHEL 7.4。 附加至 E4S 存放庫，將會針對 SAP 和 RHEL 以及基礎計算費用收取 premium。
|             | 74sap-gen2| LVM    | Linux 代理程式 | 適用于 SAP Hana 及商務應用程式的 RHEL 7.4。 第2代映射。 附加至 E4S 存放庫，將會針對 SAP 和 RHEL 以及基礎計算費用收取 premium。
|             | 7.5       | LVM    | Linux 代理程式 | 適用于 SAP Hana 及商務應用程式的 RHEL 7.5。 附加至 E4S 存放庫，將會針對 SAP 和 RHEL 以及基礎計算費用收取 premium。
|             | 75sap-gen2| LVM    | Linux 代理程式 | 適用于 SAP Hana 及商務應用程式的 RHEL 7.5。 第2代映射。 附加至 E4S 存放庫，將會針對 SAP 和 RHEL 以及基礎計算費用收取 premium。
|             | 7.6       | LVM    | Linux 代理程式 | 適用于 SAP Hana 及商務應用程式的 RHEL 7.6。 附加至 E4S 存放庫，將會針對 SAP 和 RHEL 以及基礎計算費用收取 premium。
|             | 76sap-gen2| LVM    | Linux 代理程式 | 適用于 SAP Hana 及商務應用程式的 RHEL 7.6。 第2代映射。 附加至 E4S 存放庫，將會針對 SAP 和 RHEL 以及基礎計算費用收取 premium。
|             | 7.7       | LVM    | Linux 代理程式 | 適用于 SAP Hana 及商務應用程式的 RHEL 7.7。 附加至 E4S 存放庫，將會針對 SAP 和 RHEL 以及基礎計算費用收取 premium。
RHEL-SAP-HANA | 6.7       | RAW    | Linux 代理程式 | 適用于 SAP Hana 的 RHEL 6.7。 已過期，並改用 RHEL-SAP 映射。
|             | 7.2       | LVM    | Linux 代理程式 | 適用于 SAP Hana 的 RHEL 7.2。 已過期，並改用 RHEL-SAP 映射。
|             | 7.3       | LVM    | Linux 代理程式 | 適用于 SAP Hana 的 RHEL 7.3。 已過期，並改用 RHEL-SAP 映射。
RHEL-SAP-APPS | 6.8       | RAW    | Linux 代理程式 | 適用于 SAP Business Applications 的 RHEL 6.8。 已過期，並改用 RHEL-SAP 映射。
|             | 7.3       | LVM    | Linux 代理程式 | 適用于 SAP Business Applications 的 RHEL 7.3。 已過期，並改用 RHEL-SAP 映射。
RHEL-HA       | 7.4       | LVM    | Linux 代理程式 | 具有 HA 附加元件的 RHEL 7.4。 將在基礎計算費用最上層針對 HA 和 RHEL 收取 premium。
|             | 7.5       | LVM    | Linux 代理程式 | 具有 HA 附加元件的 RHEL 7.5。 將在基礎計算費用最上層針對 HA 和 RHEL 收取 premium。
|             | 7.6       | LVM    | Linux 代理程式 | 具有 HA 附加元件的 RHEL 7.6。 將在基礎計算費用最上層針對 HA 和 RHEL 收取 premium。
RHEL-SAP-HA   | 7.4          | LVM    | Linux 代理程式 | 適用于 SAP 的 RHEL 7.4 （含 HA 和更新服務）。 附加至 E4S 存放庫。 會在基礎計算費用之上針對 SAP 和 HA 存放庫及 RHEL 收取 premium。
|             | 74sapha-gen2 | LVM    | Linux 代理程式 | 適用于 SAP 的 RHEL 7.4 （含 HA 和更新服務）。 第2代映射。 附加至 E4S 存放庫。 會在基礎計算費用之上針對 SAP 和 HA 存放庫及 RHEL 收取 premium。
|             | 7.5          | LVM    | Linux 代理程式 | 適用于 SAP 的 RHEL 7.5 （含 HA 和更新服務）。 附加至 E4S 存放庫。 會在基礎計算費用之上針對 SAP 和 HA 存放庫及 RHEL 收取 premium。
|             | 7.6          | LVM    | Linux 代理程式 | 適用于 SAP 的 RHEL 7.6 （含 HA 和更新服務）。 附加至 E4S 存放庫。 會在基礎計算費用之上針對 SAP 和 HA 存放庫及 RHEL 收取 premium。
|             | 76sapha-gen2 | LVM    | Linux 代理程式 | 適用于 SAP 的 RHEL 7.6 （含 HA 和更新服務）。 第2代映射。 附加至 E4S 存放庫。 會在基礎計算費用之上針對 SAP 和 HA 存放庫及 RHEL 收取 premium。
|             | 7.7          | LVM    | Linux 代理程式 | 適用于 SAP 的 RHEL 7.7 （含 HA 和更新服務）。 附加至 E4S 存放庫。 會在基礎計算費用之上針對 SAP 和 HA 存放庫及 RHEL 收取 premium。
|             | 77sapha-gen2 | LVM    | Linux 代理程式 | 適用于 SAP 的 RHEL 7.7 （含 HA 和更新服務）。 第2代映射。 附加至 E4S 存放庫。 會在基礎計算費用之上針對 SAP 和 HA 存放庫及 RHEL 收取 premium。
rhel-byos     |rhel-lvm74| LVM    | Linux 代理程式 | RHEL 7.4 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm75| LVM    | Linux 代理程式 | RHEL 7.5 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm76| LVM    | Linux 代理程式 | RHEL 7.6 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm76-gen2| LVM    | Linux 代理程式 | RHEL 7.6 層代 2 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm77| LVM    | Linux 代理程式 | RHEL 7.7 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm77-gen2| LVM    | Linux 代理程式 | RHEL 7.7 層代 2 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm78| LVM    | Linux 代理程式 | RHEL 7.8 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm78-gen2| LVM    | Linux 代理程式 | RHEL 7.8 層代 2 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm8 | LVM    | Linux 代理程式 | RHEL 8.0 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm8-gen2 | LVM    | Linux 代理程式 | RHEL 8.0 層代 2 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm81 | LVM    | Linux 代理程式 | RHEL 8.1 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm81-gen2 | LVM    | Linux 代理程式 | RHEL 8.1 層代 2 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm82 | LVM    | Linux 代理程式 | RHEL 8.2 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。
|             |rhel-lvm82-gen2 | LVM    | Linux 代理程式 | RHEL 8.2 層代 2 BYOS 映射（未連結到任何更新來源）不會收取 RHEL premium。

> [!NOTE]
> RHEL-SAP-HANA 產品供應專案被 Red Hat 視為生命週期結束。 現有的部署將會繼續正常運作，但 Red Hat 建議客戶從 RHEL-SAP-HANA 映射遷移至 RHEL-SAP-HA 映射，其中包括 SAP Hana 存放庫以及 HA 附加元件。 您可以 [在這裡](https://access.redhat.com/articles/3751271)找到有關 RED Hat SAP 雲端供應專案的詳細資料。

## <a name="next-steps"></a>接下來的步驟
* 深入瞭解 [Azure 中的 Red Hat 映射](./redhat-images.md)。
* 深入瞭解 [Red Hat 更新基礎結構](./redhat-rhui.md)。
* 深入瞭解 [RHEL BYOS 供應](./byos.md)專案。
* 如需所有 RHEL 版本的 Red Hat 支援原則資訊，請參閱 [Red Hat Enterprise Linux 生命週期](https://access.redhat.com/support/policy/updates/errata)頁面。
