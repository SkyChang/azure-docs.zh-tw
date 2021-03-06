---
title: IoT 隨插即用程式庫和 Sdk
description: 適用于開發 IoT 隨插即用啟用的解決方案之裝置和服務程式庫的相關資訊。
author: rido-min
ms.author: rmpablos
ms.date: 07/22/2020
ms.topic: reference
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 6ea9440c153e26e36aa17b55c4cb712dd08d4508
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2020
ms.locfileid: "92042672"
---
# <a name="microsoft-sdks-for-iot-plug-and-play"></a>適用于 IoT 隨插即用的 Microsoft Sdk

IoT 隨插即用程式庫和 Sdk 可讓開發人員在多個平臺上使用各種程式設計語言來建立 IoT 解決方案。 下表包含範例和快速入門的連結，可協助您開始使用：

## <a name="device-sdks"></a>裝置 SDK

| Language | Package | 程式碼存放庫 | 範例 | 快速入門 | 參考資料 |
|---|---|---|---|---|---|
| C-裝置 | [vcpkg 1.3。9](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/setting_up_vcpkg.md) | [GitHub](https://github.com/Azure/azure-iot-sdk-c) | [範例](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/pnp) | [連線至 IoT 中樞](quickstart-connect-device-c.md) | [參考](/azure/iot-hub/iot-c-sdk-ref/) |
| .NET-裝置 | [NuGet 1.31。0](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/) | [範例](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/device/PnpDeviceSamples) | [連線至 IoT 中樞](quickstart-connect-device-csharp.md) | [參考](/dotnet/api/microsoft.azure.devices.client?preserve-view=true&view=azure-dotnet) |
| JAVA-裝置 | [Maven 1.26。0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-device-client) | [GitHub](https://github.com/Azure/azure-iot-sdk-java/tree/master/) | [範例](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/pnp-device-sample) | [連線至 IoT 中樞](quickstart-connect-device-java.md) | [參考](/java/api/com.microsoft.azure.sdk.iot.device?preserve-view=true&view=azure-java-stable) |
| Python-裝置 | [pip 2.3。0](https://pypi.org/project/azure-iot-device/) | [GitHub](https://github.com/Azure/azure-iot-sdk-python/tree/master/) | [範例](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/pnp) | [連線至 IoT 中樞](quickstart-connect-device-python.md) | [參考](/python/api/azure-iot-device/azure.iot.device?preserve-view=true&view=azure-python) |
| 節點-裝置 | [npm 1.17。2](https://www.npmjs.com/package/azure-iot-device)  | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/master/) | [範例](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples/pnp) | [連線至 IoT 中樞](quickstart-connect-device-node.md) | [參考](/javascript/api/azure-iot-device/?preserve-view=true&view=azure-node-latest) |
| 內嵌的 C-裝置 | N/A | [GitHub](https://github.com/Azure/azure-sdk-for-c/)| [範例](howto-use-embedded-c.md#samples) | [如何使用內嵌 C](howto-use-embedded-c.md) | N/A

## <a name="service-sdks"></a>服務 SDK

| 平台  | 封裝 | 程式碼存放庫 | 範例 | 快速入門 | 參考資料 |
|---|---|---|---|---|---|
| .NET-IoT 中樞服務 | [NuGet 1.27。1](https://www.nuget.org/packages/Microsoft.Azure.Devices ) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp) | [範例](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/service/PnpServiceSamples) | N/A | [參考](/dotnet/api/microsoft.azure.devices?preserve-view=true&view=azure-dotnet) |
| JAVA-IoT 中樞服務 | [Maven 1.26。0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-service-client/1.26.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [範例](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples/pnp-service-sample) | N/A | [參考](/java/api/com.microsoft.azure.sdk.iot.service?preserve-view=true&view=azure-java-stable) |
| 節點-IoT 中樞服務 | [npm 1.13。0](https://www.npmjs.com/package/azure-iothub) | [GitHub](https://github.com/Azure/azure-iot-sdk-node) | [範例](https://github.com/Azure/azure-iot-sdk-node/tree/master/service/samples) | N/A | [參考](/javascript/api/azure-iothub/?preserve-view=true&view=azure-node-latest) |
| Python-數位 Twins 服務 | [pip 2.2。3](https://pypi.org/project/azure-iot-hub) | [GitHub](https://github.com/Azure/azure-iot-sdk-python) | [範例](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-hub/samples) | [與 IoT 中樞數位 Twins API 互動](quickstart-service-python.md) | N/A |
| 節點-數位 Twins 服務 | [npm 1.13。0](https://www.npmjs.com/package/azure-iot-digitaltwins-service) | [GitHub](https://github.com/Azure/azure-iot-sdk-node) | [範例](https://github.com/Azure/azure-iot-sdk-node/tree/master/service/samples/javascript) | [與 IoT 中樞數位 Twins API 互動](quickstart-service-node.md) | N/A |

## <a name="next-steps"></a>後續步驟

若要試用 Sdk 和程式庫，請參閱  [開發人員指南](concepts-developer-guide-device-csharp.md) 和 [裝置快速](quickstart-connect-device-c.md) 入門和 [服務快速入門](quickstart-service-node.md)。