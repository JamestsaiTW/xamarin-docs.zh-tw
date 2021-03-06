---
title: 適用于 Xamarin 的應用程式生命週期示範
description: 本檔探討 iOS 應用程式中應用程式委派所處理的各種生命週期事件, 並示範這些事件的處理時機和方式。
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/17/2018
ms.openlocfilehash: 4eefbd63a91c6fd9eeed7a6e5043db5a2ee9105b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649369"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>適用于 Xamarin 的應用程式生命週期示範

本文和[範例程式碼](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)示範 iOS 中的四個應用程式狀態, 以及在狀態`AppDelegate`變更時, 通知應用程式的方法角色。 應用程式會在應用程式變更狀態時, 將更新列印至主控台:

[![](application-lifecycle-demo-images/image3-sml.png "範例應用程式")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "應用程式會在應用程式變更狀態時, 將更新列印到主控台")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>逐步解說

1. 在**LifecycleDemo**方案中開啟**生命週期**專案。
1. `AppDelegate`開啟類別。 記錄已新增至生命週期方法, 以指出應用程式何時變更狀態:

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. 在模擬器或裝置上啟動應用程式。 `OnActivated`會在應用程式啟動時呼叫。 應用程式目前處於作用中  狀態。
1. 按模擬器或裝置上的 [首頁] 按鈕, 將應用程式帶到背景。 `OnResignActivation`和`DidEnterBackground`會在應用程式從`Active`轉換到`Inactive`並進入`Backgrounded`狀態時呼叫。 由於未將應用程式代碼集設定為在背景中執行, 因此應用程式在記憶體中被視為已_暫停_。
1. 流覽回到應用程式, 使其回到前景。 `WillEnterForeground`同時`OnActivated`也會呼叫和:

    ![](application-lifecycle-demo-images/image4.png "將狀態變更列印到主控台")

    當應用程式從背景進入前景, 並變更螢幕上顯示的文字時, 會執行 view controller 中的下列程式程式碼:

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. 按 [**首頁**] 按鈕, 將應用程式放入背景。 然後, 按兩下 [**首頁**] 按鈕來顯示應用程式切換器。 在 iPhone X 上, 從畫面底部向上輕掃:

    [![應用程式切換]器(application-lifecycle-demo-images/app-switcher-sml.png "應用程式切換")器](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. 在應用程式切換器中找出應用程式, 並向上滑動以將它移除 (在 iOS 11 上, 長按一下, 直到紅色圖示出現在角落):

    [向上![滑動以移除正在執行的應用程式]向上(application-lifecycle-demo-images/app-switcher-swipe-sml.png "滑動以移除正在執行的應用程式")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS 會終止應用程式。 請注意`WillTerminate` , 因為應用程式已在背景中_暫停_, 所以不會呼叫。

## <a name="related-links"></a>相關連結

- [LifecycleDemo (範例)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
