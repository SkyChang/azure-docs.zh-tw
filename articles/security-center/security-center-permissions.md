---
title: Azure 資訊安全中心的權限 | Microsoft Docs
description: 本文說明 Azure 資訊安全中心如何使用角色型存取控制，將權限指派給使用者，並識別每個角色允許的動作。
services: security-center
cloud: na
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/02/2020
ms.author: memildin
ms.openlocfilehash: 2a66873d558495049c97b987dfb69bbf83b4e1d8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91439486"
---
# <a name="permissions-in-azure-security-center"></a>Azure 資訊安全中心的權限

Azure 資訊安全中心會使用 [Azure 角色型存取控制 (Azure RBAC)](../role-based-access-control/role-assignments-portal.md)，以提供可在 Azure 中指派給使用者、群組與服務的[內建角色](../role-based-access-control/built-in-roles.md)。

資訊安全中心會評估資源的組態，以識別安全性問題與弱點。 在「資訊安全中心」中，當您獲指派為資源所屬的訂用帳戶或資源群組「擁有者」、「參與者」或「讀取者」角色時，您只會看到與資源相關的項目。

除了這些角色，有兩個特定的資訊安全中心角色：

* **安全性讀取者**：屬於此角色的使用者有檢視資訊安全中心的權限。 使用者可以檢視建議、警示、安全性原則和安全性狀態，但無法進行變更。
* **安全性系統管理員**：屬於此角色的使用者和「安全性讀取者」有相同的權限，而且還能更新安全性原則，以及解除警示和建議。

> [!NOTE]
> 安全性角色 (安全讀取者和安全性系統管理員) 只在資訊安全中心內有存取權。 上述安全性角色無法存取 Azure 的其他服務區域，例如儲存體、Web 和行動或物聯網。
>

## <a name="roles-and-allowed-actions"></a>角色和允許的動作

下表會顯示資訊安全中心的角色和允許的動作。

|動作|安全性讀取者/ <br> 讀取者 |安全性系統管理員  |資源群組參與者/ <br> 資源群組擁有者  |訂用帳戶參與者  |訂用帳戶擁有者  |
|:--- |:---:|:---:|:---:|:---:|:---:|
|編輯安全性原則|-|✔|-|-|✔|
|新增/指派方案 (包括法規遵循標準)|-|-|-|-|✔|
|啟用/停用 Azure Defender|-|✔|-|-|✔|
|啟用/停用自動佈建|-|✔|-|✔|✔|
|針對資源套用安全性建議</br> (並使用[快速修正！](security-center-remediate-recommendations.md#quick-fix-remediation))|-|-|✔|✔|✔|
|解除警示|-|✔|-|✔|✔|
|檢視警示和建議|✔|✔|✔|✔|✔|

> [!NOTE]
> 我們建議您指派所需的最寬鬆角色，以便使用者完成其工作。 例如，將「讀取者」角色指派給只需要檢視資源安全性狀態的相關資訊，但不採取行動的使用者，例如套用建議或編輯原則。
>
>

## <a name="next-steps"></a>後續步驟
本文說明資訊安全中心如何使用 RBAC，將權限指派給使用者，並識別每個角色允許的動作。 現在，您已熟悉監視您的訂用帳戶的安全性狀態所需的角色指派，編輯安全性原則和套用建議，接著了解如何︰

- [在資訊安全中心設定安全性原則](tutorial-security-policy.md)
- [管理資訊安全中心的安全性建議](security-center-recommendations.md)
- [監視您的 Azure 資源的安全性健全狀況](security-center-monitoring.md)
- [在資訊安全中心管理和回應安全性警示](security-center-managing-and-responding-alerts.md)
- [監視合作夥伴安全性解決方案](security-center-partner-solutions.md)
