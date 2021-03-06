---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5a06a0663601c221dd456b9cf4437cb9f32a18f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "81399980"
---
:::row:::
    :::column span="3":::
        C + + 語音 SDK 可在 Windows、Linux 和 macOS 上取得。 如需詳細資訊，請參閱<a href="https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech" target="_blank">CognitiveServices <span class="docon docon-navigate-external x-hidden-focus"></span> </a>。
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="C++" src="https://docs.microsoft.com/media/logos/logo_Cplusplus.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

#### <a name="c-nuget-package"></a>C + + NuGet 套件

您可以使用下列命令，從 **封裝管理員** 安裝 c + + 語音 SDK `Install-Package` 。

```powershell
Install-Package Microsoft.CognitiveServices.Speech
```

#### <a name="c-binaries-and-header-files"></a>C + + 二進位檔和標頭檔

或者，您可以從二進位檔安裝 c + + 語音 SDK。 將 SDK 下載為<a href="https://aka.ms/csspeech/linuxbinary" target="_blank">tar 套件 <span class="docon docon-navigate-external x-hidden-focus"></span> </a> ，並在您選擇的目錄中解壓縮檔案。 此封裝的內容 (，其中包含 x86 和 x64 目標架構的標頭檔) 結構如下：

  | Path                   | 描述                                          |
  |------------------------|------------------------------------------------------|
  | `license.md`           | 授權                                              |
  | `ThirdPartyNotices.md` | 協力廠商通知                                  |
  | `include`              | C + + 的標頭檔                                 |
  | `lib/x64`              | 與應用程式連結的原生 x64 程式庫 |
  | `lib/x86`              | 與應用程式連結的原生 x86 程式庫 |

  若要建立應用程式，請將所需的二進位檔 (和程式庫) 複製或移至您的開發環境。 視需要將它們包含在您的建置程序中。

#### <a name="additional-resources"></a>其他資源

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp" target="_blank">Windows、Linux 和 macOS 快速入門 c + + 原始程式碼 <span class="docon docon-navigate-external x-hidden-focus"></span></a>