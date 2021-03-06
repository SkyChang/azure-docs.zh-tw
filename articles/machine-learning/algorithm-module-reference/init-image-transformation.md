---
title: 初始映射 Transformationply 映射轉換
titleSuffix: Azure Machine Learning
description: 瞭解如何使用初始映射轉換模組來初始化影像轉換。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: aa81987f9214870e248ef9b625e6afcd1093fe5d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "90907886"
---
# <a name="init-image-transformation"></a>初始化映像轉換

本文描述如何使用 Azure Machine Learning 設計工具中的 [ **初始化影像轉換** ] 模組，以初始化影像轉換，以指定要如何轉換影像。

## <a name="how-to-configure-init-image-transformation"></a>如何設定初始映射轉換

1.  在設計工具中，將 **Init 映射轉換** 模組新增至您的管線。 

2.  針對 [重 **設**大小]，指定是否要將輸入 PIL 影像的大小調整為指定的大小。 如果您選擇 [True]，則可以指定所需的輸出影像大小 **，預設**為256。 

3.  針對 [ **置中裁剪**]，指定是否要在中間裁剪指定的 PIL 影像。 如果您選擇 [True]，您可以在 **裁剪大小**中指定裁剪大小所需的輸出影像大小，預設為224。  

4.  針對 [ **pad**]，指定是否要在所有側邊以板值0填補指定的 PIL 影像。 如果您選擇 [True]，則可以指定填補 (要在 **填補**的每個框線上新增) 的圖元數。

5.  針對 **色彩抖動**，請指定是否要隨機變更影像的亮度、對比和飽和度。

6.  若為 **灰階**，請指定是否要將影像轉換成灰階。

7.  針對 **隨機**調整大小的裁剪，指定是否要將指定的 PIL 影像裁剪成隨機大小和外觀比例。 隨機大小的裁剪 (範圍從0.08 到 1.0) 原始大小，以及隨機的外觀比例 (範圍從3/4 到 4/3) 的原始外觀比例。 這項裁剪最後會調整大小為指定的大小。
    這通常用於定型開始網路。 如果您選擇 [True]，就可以依預設值指定每個邊緣的預期輸出 **大小（預設**為256）。

8.  針對 **隨機裁剪**，指定是否要在隨機位置裁剪指定的 PIL 影像。 如果您選擇 [True]，您可以根據預設的裁剪 **大小**，指定所需的裁剪輸出大小（預設為224）。

9.  若為 **隨機水準翻轉**，請指定是否要在機率為0.5 的情況下，以隨機方式垂直翻轉指定的 PIL 影像。

10.  若為 **隨機垂直翻轉**，請指定是否要使用機率0.5 來隨機垂直翻轉指定的 PIL 影像。

11.  如果是 **隨機旋轉**，請指定是否要依角度旋轉影像。 如果您選擇 [True]，您可以藉由設定 **隨機旋轉度數**來指定度數範圍，這表示預設值為0，表示 ( 度、+ 度) 。

12.  針對 [ **隨機仿射**]，指定是否要將影像的隨機仿射轉換保持置中不變。 如果您選擇 [True]，您可以在 [ **隨機仿射度**] 中指定要從中選取的度數範圍，這表示預設值為 0 ( 度、+ 度) 。

13.  若為 **隨機灰階**，請指定是否要將影像隨機轉換成具有機率0.1 的灰階。

14.  若為 **隨機觀點**，請指定是否要在機率為0.5 的情況下，隨機執行給定 PIL 影像的透視圖轉換。


16.  連接至套用 [影像轉換](apply-image-transformation.md) 模組，以將上述指定的轉換套用至輸入映射資料集。

17. 提交管線。

## <a name="results"></a>結果

轉換完成之後，您可以在 [套用 [影像轉換](apply-image-transformation.md) ] 模組的輸出中找到已轉換的影像。


## <a name="technical-notes"></a>技術說明  

如需 [https://pytorch.org/docs/stable/torchvision/transforms.html](https://pytorch.org/docs/stable/torchvision/transforms.html) 影像轉換的詳細資訊，請參閱。

###  <a name="module-parameters"></a>模組參數  

| 名稱                    | 範圍   | 類型    | 預設 | 描述                              |
| ----------------------- | ------- | ------- | ------- | ---------------------------------------- |
| 調整大小                  | 任意     | 布林值 | True    | 將輸入 PIL 影像的大小調整為指定的大小 |
| 大小                    | >= 1     | 整數 | 256     | 指定所需的輸出大小          |
| 中心裁剪             | 任意     | 布林值 | True    | 裁剪中央的指定 PIL 影像  |
| 裁剪大小               | >= 1     | 整數 | 224     | 指定所需的裁剪輸出大小 |
| Pad                     | 任意     | 布林值 | 否   | 使用指定的「pad」值，在所有邊填補指定的 PIL 影像 |
| 填補                 | >= 0     | 整數 | 0       | 每個框線的填補                   |
| 色彩抖動            | 任意     | 布林值 | 否   | 隨機變更影像的亮度、對比和飽和度 |
| 灰階               | 任意     | 布林值 | 否   | 將影像轉換成灰階               |
| 隨機調整大小的裁剪     | 任意     | 布林值 | 否   | 將指定的 PIL 影像裁剪成隨機大小和外觀比例 |
| 隨機大小             | >= 1     | 整數 | 256     | 每個邊緣的預期輸出大小        |
| 隨機裁剪             | 任意     | 布林值 | 否   | 在隨機位置裁剪指定的 PIL 影像 |
| 隨機裁剪大小        | >= 1     | 整數 | 224     | 所需的裁剪輸出大小          |
| 隨機水準翻轉  | 任意     | 布林值 | True    | 使用指定的機率，以隨機方式垂直翻轉指定的 PIL 影像 |
| 隨機垂直翻轉    | 任意     | 布林值 | 否   | 以指定的機率隨機垂直翻轉指定的 PIL 影像 |
| 隨機旋轉         | 任意     | 布林值 | 否   | 依角度旋轉影像                |
| 隨機旋轉度 | [0180] | 整數 | 0       | 要從中選取的度數範圍          |
| 隨機仿射           | 任意     | 布林值 | 否   | 影像的隨機仿射轉換保持置中不變 |
| 隨機仿射度   | [0180] | 整數 | 0       | 要從中選取的度數範圍          |
| 隨機灰階        | 任意     | 布林值 | 否   | 使用機率0.1 隨機將影像轉換為灰階 |
| 隨機觀點      | 任意     | 布林值 | 否   | 使用機率0.5 隨機執行給定 PIL 影像的透視圖轉換 |
| 隨機清除          | 任意     | 布林值 | 否   | 隨機選取影像中的矩形區域，並清除其具有機率0.5 的圖元 |

###  <a name="output"></a>輸出  

| 名稱                        | 類型                    | 描述                              |
| --------------------------- | ----------------------- | ---------------------------------------- |
| 輸出影像轉換 | TransformationDirectory | 可以連線以套用 **影像轉換** 模組的輸出影像轉換。 |

## <a name="next-steps"></a>後續步驟

請參閱 Azure Machine Learning 的[可用模組集](module-reference.md)。 