---
title: 依區域的資源可用性
description: Azure 容器執行個體服務的計算和記憶體資源在不同 Azure 區域中的可用性。
ms.topic: article
ms.date: 04/27/2020
ms.openlocfilehash: 97baa5199a1803bd967c0b55c846908ea5a2ddcf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89565424"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Azure 容器執行個體在 Azure 區域中的資源可用性

本文詳細說明 azure 容器實例計算、記憶體和儲存體資源在 Azure 區域中的可用性，以及目標作業系統的可用性。 

顯示的值為每個[容器群組](container-instances-container-groups.md)部署可用的最大資源。 以下是本文章發行時的值。 

> [!NOTE]
> 在這些資源限制下建立的容器群組會受限於部署區域的可用性。 地區負載過重時，您在部署執行個體時可能會失敗。 若要減輕這類部署失敗的問題，請嘗試部署資源設定較低的實例，或稍後或在具有可用資源的不同區域中試用您的部署。

如需部署中的配額和其他限制的相關資訊，請參閱 [Azure 容器執行個體的配額和限制](container-instances-quotas.md)。

## <a name="availability---general"></a>可用性 - 一般

適用于 Linux 和 [支援](container-instances-faq.md#what-windows-base-os-images-are-supported) 的 Windows Server 2016 容器的容器群組可使用下欄區域和資源上限。

| 區域 | OS | 最大 CPU | 最大記憶體 (GB)  | 儲存體 (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| 巴西南部、加拿大中部、印度中部、美國中部、東亞、美國東部、美國東部2、歐洲北部、美國中南部、東南亞、印度南部、英國南部、西歐、美國西部、美國西部2 | Linux | 4 | 16 | 50 |
| 澳大利亞東部、日本東部 | Linux | 2 | 8 | 50 |
| 美國中北部 | Linux | 2 | 3.5 | 50 |
| 巴西南部、日本東部、西歐 | Windows | 4 | 16 | 20 |
| 美國東部、美國西部 | Windows | 4 | 14 | 20 |
| 澳大利亞東部、加拿大中部、印度中部、美國中部、東亞、美國東部2、美國中北部、歐洲北部、美國中南部、東南亞、印度南部、英國南部美國西部2 | Windows | 2 | 3.5 | 20 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>可用性 - Windows Server 2019 LTSC，1809年部署 (預覽)

下欄區域和資源上限可供具有 Windows Server 2019 容器的容器群組使用 (預覽版) 。

| 區域 | OS | 最大 CPU | 最大記憶體 (GB)  | 儲存體 (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| 澳大利亞東部、巴西南部、加拿大中部、印度中部、美國中部、東亞、美國東部、日本東部、美國中北部、歐洲北部、美國中南部、東南亞、印度南部、英國南部、西歐 | Windows | 4 | 16 | 20 |
| 美國東部2，美國西部2 | Windows | 2 | 3.5 | 20 |


## <a name="availability---virtual-network-deployment"></a>可用性-虛擬網路部署

[Azure 虛擬網路](container-instances-vnet.md)中部署的容器群組可使用下欄區域和資源上限。

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>可用性 - GPU 資源 (預覽)

下欄區域和資源上限可供使用 [GPU 資源](container-instances-gpu.md) 部署的容器群組 (預覽) 。

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>後續步驟

如果您想要了解其他區域或增加的資源可用性，請經由 [aka.ms/aci/feedback](https://aka.ms/aci/feedback) 告知小組人員。

如需對容器執行個體部署疑難排解的相關資訊，請參閱[對 Azure 容器執行個體的部署問題進行疑難排解](container-instances-troubleshooting.md)。


[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
