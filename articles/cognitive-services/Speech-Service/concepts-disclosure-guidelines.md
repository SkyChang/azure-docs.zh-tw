---
title: 洩漏設計指導方針
titleSuffix: Azure Cognitive Services
description: 公開設計指導方針和評估洩漏等級的簡介。
services: cognitive-services
author: sharonlo101
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: angle
ms.openlocfilehash: fe38c6b7cfb1abbaf3f1079dd8bff66b51b98091
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "74776380"
---
# <a name="disclosure-design-guidelines"></a>公開設計指引
瞭解如何透過清楚瞭解語音體驗的綜合本質，來建立及維護與客戶的信任。

## <a name="what-is-disclosure"></a>什麼是洩漏？

洩漏是讓人們知道他們&#39;重新與合成方式產生的聲音互動或接聽的方法。

## <a name="why-is-disclosure-necessary"></a>為什麼需要洩漏？

需要洩漏電腦產生的語音的綜合來源，是相當新的。 在過去，電腦產生的聲音顯然很明顯，也就是，沒有人會犯錯。 不過，每一天，綜合語音的真實性也會改善，而且與人類語音的區別更加容易。

## <a name="goals"></a>目標
以下是設計綜合語音體驗時要牢記在心的原則：

**強化信任**
<br>利用 Turing 測試失敗的意圖進行設計，而不會降低體驗。 讓使用者與綜合語音互動，同時讓他們順暢地與經驗互動。

**適應使用的內容**
<br>瞭解您的使用者將如何與綜合語音互動，以在正確的時間提供正確的洩漏類型。

**設定明確的期望**
<br>讓使用者可以輕鬆地探索和瞭解代理程式的功能。 提供在要求時深入瞭解綜合語音技術的機會。

**接納失敗**
<br>使用失敗的時間來強化代理程式的功能。

## <a name="how-to-use-this-guide"></a>如何使用本指南

本指南可協助您判斷哪些洩漏模式最適合您的綜合語音體驗。 接著，我們會提供使用方式和時機的範例。 上述每一種模式都是設計用來將綜合語音的使用者最大化，同時保持為人為中心的設計。

考慮到豐富的語音體驗設計指引，我們特別著重于：

1. [**洩漏評**](#disclosure-assessment)量：判斷您的綜合語音體驗所建議之洩漏類型的流程

2. [**如何公開**](concepts-disclosure-patterns.md)：適用于綜合語音體驗的洩漏模式範例

3. [**何時要公開**](concepts-disclosure-patterns.md#when-to-disclose)：在整個使用者旅程圖中取得最佳的時間

## <a name="disclosure-assessment"></a>洩漏評量
請考慮您的使用者&#39; 對互動的期望，以及他們將在其中體驗語音的內容。 如果內容清楚說出綜合語音 &quot; ，則 &quot; 洩漏可能很短、短暫或甚至不需要。 影響洩漏的主要內容類型包括角色類型、案例類型和暴露程度。 也有助於考慮誰可能正在接聽。

### <a name="understand-context"></a>了解內容

您可以使用此工作表來判斷綜合語音體驗的內容。 您將在下一個步驟中套用此項，以決定您的洩漏層級。

|                                    | 使用的內容                                                                                                                                                                                                                                                                                                                                                       | 潛在的風險 & 挑戰                                                                                                                                                                                                                                                                                                                                                                       |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1. 角色類型**               | **如果有下列任何一項，您的角色就會符合「人類贊的角色」類別：**<br><br><ul><li> 無論是虛構的人，角色都能表達真實的人。  (例如，相片或電腦產生的真實人員呈現) <br><br><li> 綜合聲音是以廣泛辨識的真實人員語音為基礎 (例如名人、政治圖)  | 您提供給您的角色越類似人類的標記法，使用者越可能將它與真實人員相關聯，或讓他們相信內容是由真實人員（而非電腦所產生）說出。 </ul>                                                                                                                                                                      |
| **2. 案例類型**            | **如果有下列任何一項，則您的語音體驗符合「敏感性」類別：**<br><br><ul><li> 取得或顯示使用者的個人資訊 <br><br> <li> 廣播時間敏感的新聞/資訊 (例如緊急警示) <br><br><li> 旨在協助真實人員彼此通訊 (例如，讀取個人電子郵件/文字) <br><br> <li> 提供醫療/健康狀態協助 </ul>            | 當主題與機密、個人或緊急相關時，使用綜合語音可能不會覺得適合或值得信任的人。 他們可能也會期望與真實人相同的理解程度和內容感知。 |
| **3. 暴露程度** |**在下列情況下，您的語音體驗最可能符合「高」類別：** <br><br><ul><li>使用者會頻繁地聆聽或與綜合語音互動，或長時間進行互動 </ul>                                                                                                                                                                             | 建立長期關聯性時，與使用者的透明和建立信任的重要性更高。                                                                                                                                                                                                                                                                      |

### <a name="determine-disclosure-level"></a>判斷洩漏等級

使用下圖來根據您的使用方式，判斷您的綜合語音體驗是否需要高或低的洩漏。

  ![洩漏評量圖表](media/responsible-ai/disclosure-guidelines/flowchart.png)

## <a name="reference-docs"></a>參考文件

* [配音員的洩漏](https://aka.ms/disclosure-voice-talent)
* [綜合語音技術的負責任部署指導方針](concepts-guidelines-responsible-deployment-synthetic.md)
* [管制總覽](concepts-gating-overview.md)

## <a name="next-steps"></a>後續步驟

* [公開設計模式](concepts-disclosure-patterns.md)
