---
title: 在點陣地圖上轉譯自訂資料 |Microsoft Azure 對應
description: 瞭解如何將圖釘、標籤和幾何圖形新增至點陣地圖。 請參閱如何在 Azure 地圖服務中使用靜態映射服務以進行此用途。
author: anastasia-ms
ms.author: v-stharr
ms.date: 01/23/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: ec62461e5a12f0c566becdfc7d9a1464433ee656
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91311014"
---
# <a name="render-custom-data-on-a-raster-map"></a>在點陣地圖上轉譯自訂資料

本文說明如何使用 [靜態映射服務](https://docs.microsoft.com/rest/api/maps/render/getmapimage)（具有影像組合功能），以允許在地圖上進行重迭。 影像組合可讓您使用其他資料（例如自訂圖釘、標籤和幾何重迭）來重新顯示點陣磚。

若要呈現自訂圖釘、標籤和幾何重迭，您可以使用 Postman 應用程式。 您可以使用 Azure 地圖服務 [資料服務 api](https://docs.microsoft.com/rest/api/maps/data) 來儲存和轉譯重迭。

> [!Tip]
> 使用 Azure 地圖服務 Web SDK 在網頁上顯示簡單的地圖，而不是使用靜態映射服務，通常會更加符合成本效益。 Web SDK 會使用地圖底圖，且除非使用者移動流覽及縮放地圖，否則通常只會在每個地圖載入時產生一小部分的交易。 請注意，Azure 地圖服務 web SDK 有停用移動流覽和縮放的選項。 此外，Azure 地圖服務 web SDK 提供比靜態地圖 web 服務更豐富的資料視覺效果選項組。  

## <a name="prerequisites"></a>Prerequisites

### <a name="create-an-azure-maps-account"></a>建立 Azure 地圖服務帳戶

若要完成本文中的程式，您必須先建立 Azure 地圖服務帳戶，並取得地圖服務帳戶金鑰。 依照 [建立帳戶](quick-demo-map-app.md#create-an-azure-maps-account) 中的指示建立 Azure 地圖服務帳戶訂用帳戶，並依照 [取得主要金鑰](quick-demo-map-app.md#get-the-primary-key-for-your-account) 中的步驟取得帳戶的主要金鑰。 如需 Azure 地圖服務中驗證的詳細資訊，請參閱[管理 Azure 地圖服務中的驗證](./how-to-manage-authentication.md)。


## <a name="render-pushpins-with-labels-and-a-custom-image"></a>使用標籤和自訂影像呈現圖釘

> [!Note]
> 本節中的程式需要定價層 S0 或 S1 中的 Azure 地圖服務帳戶。

Azure 地圖服務帳戶 S0 層僅支援參數的單一實例 `pins` 。 它可讓您使用自訂影像，轉譯最多五個圖釘（在 URL 要求中指定）。

若要使用標籤和自訂影像來呈現圖釘，請完成下列步驟：

1. 建立用來儲存要求的集合。 在 Postman 應用程式中，選取 [ **新增**]。 在 [新建] 視窗中，選取 [集合]。 為集合命名，然後選取 [建立] 按鈕。 

2. 若要建立要求，請再次選取 [新增]。 在 [新建] 視窗中，選取 [要求]。 輸入圖釘的 **要求名稱** 。 選取您在上一個步驟中建立的集合，做為儲存要求的位置。 然後選取 [儲存]。
    
    ![在 Postman 中建立要求](./media/how-to-render-custom-data/postman-new.png)

3. 在 [建立器] 索引標籤上選取 [取得 HTTP 方法]，並輸入下列 URL 以建立 GET 要求。

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.98,%2040.77&pins=custom%7Cla15+50%7Cls12%7Clc003b61%7C%7C%27CentralPark%27-73.9657974+40.781971%7C%7Chttps%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2FAzureMapsCodeSamples%2Fmaster%2FAzureMapsCodeSamples%2FCommon%2Fimages%2Ficons%2Fylw-pushpin.png
    ```
    以下是產生的映射：

    ![具有標籤的自訂圖釘](./media/how-to-render-custom-data/render-pins.png)


## <a name="get-data-from-azure-maps-data-storage"></a>從 Azure 地圖服務資料儲存體取得資料

> [!Note]
> 本節中的程式需要定價層 S1 中的 Azure 地圖服務帳戶。

您也可以使用 [資料上傳 API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)來取得路徑和 pin 位置資訊。 遵循下列步驟來上傳路徑和圖釘資料。

1. 在 Postman 應用程式中，開啟您在上一節建立的集合中的新索引標籤。 在 [建立器] 索引標籤上選取 POST HTTP 方法，然後輸入下列 URL 以提出 POST 要求：

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

2. 在 [ **參數** ] 索引標籤上，輸入下列索引鍵/值組，用於 POST 要求 URL。 將 `subscription-key` 值取代為您 Azure 地圖服務的訂用帳戶金鑰。
    
    ![Postman 中的索引鍵/值參數](./media/how-to-render-custom-data/postman-key-vals.png)

3. 在 [ **主體** ] 索引標籤上，選取原始輸入格式，並從下拉式清單中選擇 [JSON] 作為輸入格式。 提供此 JSON 作為要上傳的資料：
    
    ```JSON
    {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -73.98235,
                  40.76799
                ],
                [
                  -73.95785,
                  40.80044
                ],
                [
                  -73.94928,
                  40.7968
                ],
                [
                  -73.97317,
                  40.76437
                ],
                [
                  -73.98235,
                  40.76799
                ]
              ]
            ]
          }
        },
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "LineString",
            "coordinates": [
              [
                -73.97624731063843,
                40.76560773817073
              ],
              [
                -73.97914409637451,
                40.766826609362575
              ],
              [
                -73.98513078689575,
                40.7585866048861
              ]
            ]
          }
        }
      ]
    }
    ```

4. 選取 [ **傳送** 和審核回應標頭]。 要求成功時，[位置] 標頭將會包含狀態 URI，用以查看上傳要求目前的狀態。 狀態 URI 的格式如下。  

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

5. 複製您的狀態 URI，並將訂用帳戶金鑰參數附加至您的 Azure 地圖服務帳戶訂用帳戶金鑰的值。 使用您用來上傳資料的相同帳戶訂用帳戶金鑰。 狀態 URI 格式看起來應該如下所示：

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

6. 若要取得 udId，請在 Postman 應用程式中開啟新的索引標籤。 選取 [建立器] 索引標籤上的 [取得 HTTP 方法]。在狀態 URI 提出 GET 要求。 如果您的資料上傳成功，您將會在回應本文中收到 udId。 複製 udId。

   ```JSON
   {
      "udid" : "{udId}"
   }
   ```

7. 使用 `udId` 從資料上傳 API 收到的值來轉譯地圖上的功能。 若要這樣做，請在您于上一節中建立的集合中開啟新的索引標籤。 在 [建立器] 索引標籤上選取 [取得 HTTP 方法]，將 {訂用帳戶金鑰} 和 {udId} 取代為您的值，然後輸入此 URL 以提出 GET 要求：

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.96682739257812%2C40.78119135317995&pins=default|la-35+50|ls12|lc003C62|co9B2F15||'Times Square'-73.98516297340393 40.758781646381024|'Central Park'-73.96682739257812 40.78119135317995&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.30||udid-{udId}
    ```

    以下是回應影像：

    ![從 Azure 地圖服務資料儲存體取得資料](./media/how-to-render-custom-data/uploaded-path.png)

## <a name="render-a-polygon-with-color-and-opacity"></a>呈現色彩和不透明度的多邊形

> [!Note]
> 本節中的程式需要定價層 S1 中的 Azure 地圖服務帳戶。


您可以使用樣式修飾詞搭配[路徑參數](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters)來修改多邊形的外觀。

1. 在 Postman 應用程式中，開啟您稍早建立的集合中的新索引標籤。 在 [建立器] 索引標籤上選取 [取得 HTTP 方法]，並輸入下列 URL 來設定 GET 要求以轉譯具有色彩和不透明度的多邊形：
    
    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&sku=S1&zoom=14&height=500&Width=500&center=-74.040701, 40.698666&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.50||-74.03995513916016 40.70090237454063|-74.04082417488098 40.70028420372218|-74.04113531112671 40.70049568385827|-74.04298067092896 40.69899904076542|-74.04271245002747 40.69879568992435|-74.04367804527283 40.6980961582905|-74.04364585876465 40.698055487620714|-74.04368877410889 40.698022951066996|-74.04168248176573 40.696444909137|-74.03901100158691 40.69837271818651|-74.03824925422668 40.69837271818651|-74.03809905052185 40.69903971085914|-74.03771281242369 40.699340668780984|-74.03940796852112 40.70058515602143|-74.03948307037354 40.70052821920425|-74.03995513916016 40.70090237454063
    &subscription-key={subscription-key}
    ```

    以下是回應影像：

    ![轉譯不透明的多邊形](./media/how-to-render-custom-data/opaque-polygon.png)


## <a name="render-a-circle-and-pushpins-with-custom-labels"></a>使用自訂標籤呈現圓形和圖釘

> [!Note]
> 本節中的程式需要定價層 S1 中的 Azure 地圖服務帳戶。


您可以藉由新增樣式修飾詞來修改 pin 的外觀。 例如，若要使圖釘和其標籤變大或較小，請使用 `sc` "scale style" 修飾詞。 此修飾詞會採用大於零的值。 1 的值是標準比例。 大於 1 的值會使圖釘變大，小於 1 的值會使它們變小。 如需樣式修飾詞的詳細資訊，請參閱 [靜態映射服務路徑參數](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters)。


請遵循下列步驟，使用自訂標籤來呈現圓形和圖釘：

1. 在 Postman 應用程式中，開啟您稍早建立的集合中的新索引標籤。 在 [建立器] 索引標籤上選取 [取得 HTTP 方法]，並輸入此 URL 以提出 GET 要求：

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co002D62||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

    以下是回應影像：

    ![使用自訂圖釘呈現圓形](./media/how-to-render-custom-data/circle-custom-pins.png)

2. 若要變更上一個步驟中圖釘的色彩，請變更 "co" 樣式修飾詞。 查看 `pins=default|la15+50|al0.66|lc003C62|co002D62|` ，目前的色彩會指定為 CSS 中的 #002D62。 假設您想要將它變更為 #41d42a。 在 "co" 規範後面寫入新的色彩值，如下所示： `pins=default|la15+50|al0.66|lc003C62|co41D42A|` 。 提出新的 GET 要求：

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co41D42A||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

    以下是變更釘選色彩之後的回應影像：

    ![以更新的圖釘呈現圓形](./media/how-to-render-custom-data/circle-updated-pins.png)

同樣地，您可以變更、新增和移除其他樣式修飾詞。

## <a name="next-steps"></a>後續步驟


* 探索 [Azure 地圖服務 Get Map Image API](https://docs.microsoft.com/rest/api/maps/render/getmapimage) \(英文\) 文件。
* 若要深入瞭解 Azure 地圖服務資料服務，請參閱 [服務檔](https://docs.microsoft.com/rest/api/maps/data)。

