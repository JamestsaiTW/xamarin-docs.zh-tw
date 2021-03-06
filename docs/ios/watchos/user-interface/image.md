---
title: Xamarin 中的 watchOS 影像控制項
description: 本檔說明如何在以 Xamarin 建立的 watchOS 應用程式中使用影像控制項。 其中討論 WKInterfaceImage 控制項、SetImage 方法、將影像新增至監看式擴充功能、動畫等等。
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 7ff97f27a89b9943194ea875458f4e63f7797b76
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645845"
---
# <a name="watchos-image-controls-in-xamarin"></a>Xamarin 中的 watchOS 影像控制項

提供 watchOS [`WKInterfaceImage`](xref:WatchKit.WKInterfaceImage)控制項來顯示影像和簡單的動畫。 有些控制項也可以具有背景影像 (例如按鈕、群組和介面控制器)。

![](image-images/image-walkway.png "Apple Watch 顯示圖片") ![](image-images/image-animation.png "Apple Watch 的簡單動畫")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

使用資產類別目錄映射來新增影像以監看套件應用程式。
只有 **@2x** 版本是必要的因為所有監看 Retina 顯示器的裝置。

![](image-images/asset-universal-sml.png "只有2x 版本是必要的, 因為所有監看式裝置都有 Retina 顯示")

最好的作法是確保影像本身是監看顯示的正確大小。 *避免*使用大小不正確的影像 (特別是大型映射) 和縮放比例, 以顯示在監看式上。

您可以使用資產目錄影像中的監看套件大小 (38mm 和 42mm), 為每個顯示大小指定不同的影像。

![](image-images/asset-watch-sml.png "您可以使用資產目錄影像中的監看套件大小38mm 和 42mm, 為每個顯示大小指定不同的影像")


## <a name="images-on-the-watch"></a>監看式上的影像

顯示影像最有效率的方式是將*它們包含在 watch 應用程式專案中*, 並使用`SetImage(string imageName)`方法來顯示它們。

例如, [WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog/)範例在監看式應用程式專案中, 已將許多影像新增至資產目錄:

![](image-images/asset-whale-sml.png "WatchKitCatalog 範例在監看式應用程式專案中, 已將許多影像新增至資產目錄")

這些專案可以有效率地載入, 並在 watch `SetImage`上以字串名稱參數顯示:

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>背景影像

相同`SetBackgroundImage (string imageName)`的邏輯適用于`Button`、 `Group`和`InterfaceController`類別上的。 藉由將影像儲存在監看式應用程式本身來達到最佳效能。


## <a name="images-in-the-watch-extension"></a>監看式擴充功能中的影像

除了載入儲存在 watch 應用程式本身的影像之外, 您還可以將延伸模組配套中的影像傳送至監看式應用程式以供顯示 (或者, 您可以從遠端位置下載影像, 並加以顯示)。

若要從監看式擴充功能載入`UIImage`影像, 請建立`SetImage`實例, `UIImage`然後使用物件呼叫。

例如, [WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)範例在 watch 擴充功能專案中具有名為**黃蜂**的映射:

![](image-images/asset-bumblebee-sml.png "WatchKitCatalog 範例在 watch 擴充功能專案中具有名為黃蜂的映射")

下列程式碼將會產生:

- 正在載入記憶體中的影像, 以及
- 顯示在監看式上。

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>Animations

若要建立一組影像的動畫, 它們的開頭都應該是相同的前置詞, 並具有數值尾碼。

[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)範例在 watch 應用程式專案中有一系列具有**匯流排**前置詞的已編號影像:

![](image-images/asset-bus-animation-sml.png "WatchKitCatalog 範例在 watch 應用程式專案中有一系列具有匯流排前置詞的已編號影像")

若要將這些影像顯示為動畫, 請先使用`SetImage`加上前置詞名稱來載入影像, 然後再呼叫: `StartAnimating`

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

在`StopAnimating` image 控制項上呼叫以停止動畫迴圈:

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>附錄：快取影像 (watchOS 1)

> [!IMPORTANT]
> watchOS 3 應用程式會完全在裝置上執行。 下列資訊僅適用于 watchOS 1 應用程式。

如果應用程式重複使用儲存在擴充功能中的映射 (或已下載), 您可以在監看式的儲存體中快取影像, 以提高後續顯示器的效能。

`WKInterfaceDevice`使用 s `AddCachedImage`方法將影像傳送至監看式, 然後使用`SetImage`搭配 image name 參數作為字串來顯示它:

```csharp
var device = WKInterfaceDevice.CurrentDevice;
using (var image = UIImage.FromBundle ("Bumblebee")) {
    if (!device.AddCachedImage (image, "Bumblebee")) {
            Console.WriteLine ("Image cache full.");
        } else {
            cachedImage.SetImage ("Bumblebee");
        }
    }
}
```

您可以使用`WKInterfaceDevice.CurrentDevice.WeakCachedImages`, 在程式碼中查詢影像快取的內容。


### <a name="managing-the-cache"></a>管理快取

快取的大小約為 20 MB。 它會在應用程式重新開機時保持不變, 而當它填滿時, 您必須`RemoveCachedImage`負責`RemoveAllCachedImages`在`WKInterfaceDevice.CurrentDevice`物件上使用或方法清除檔案。



## <a name="related-links"></a>相關連結

- [WatchKitCatalog （範例）](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple 的影像檔](https://developer.apple.com/documentation/watchkit/wkinterfaceimage)
