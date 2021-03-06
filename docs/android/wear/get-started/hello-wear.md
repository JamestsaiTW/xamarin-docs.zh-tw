---
title: Hello, Wear
description: 建立您的第一個 Android 磨損應用程式, 並在磨損模擬器或裝置上執行。 本逐步解說提供逐步指示, 說明如何建立小型的 Android 磨損專案來處理按鈕的點擊, 並在磨損裝置上顯示按一下計數器。 它說明如何使用磨損模擬器或透過藍牙連線到 Android 手機的磨損裝置, 來對應用程式進行 debug。 它也提供一組適用于 Android 磨損的偵錯工具提示。
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/10/2018
ms.openlocfilehash: 056ab7a9fe4bcb7f07a9a7cd7c841a3d9f7574b6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648017"
---
# <a name="hello-wear"></a>Hello, Wear

_建立您的第一個 Android 磨損應用程式, 並在磨損模擬器或裝置上執行。本逐步解說提供逐步指示, 說明如何建立小型的 Android 磨損專案來處理按鈕的點擊, 並在磨損裝置上顯示按一下計數器。它說明如何使用磨損模擬器或透過藍牙連線到 Android 手機的磨損裝置, 來對應用程式進行 debug。它也提供一組適用于 Android 磨損的偵錯工具提示。_

![要在本教學課程中完成之磨損應用程式的螢幕擷取畫面](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>您的第一個磨損應用程式

請遵循下列步驟來建立您的第一個 Xamarin. Android 磨損應用程式:

### <a name="1-create-a-new-android-project"></a>1.建立新的 Android 專案

建立新的**Android 磨損應用程式**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![在 [新增專案] 對話方塊中建立新的 Android 磨損應用程式](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![在 [新增方案] 對話方塊中建立新的 Android 磨損應用程式](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


此範本會自動包含**Xamarin Android 穿戴式 Library** NuGet (和相依性), 讓您可以存取特定的 widget。 如果您看不到 [磨損] 範本, 請參閱[安裝和設定](~/android/wear/get-started/installation.md)指南, 再次檢查您是否已安裝支援的 Android SDK。 

### <a name="2-choose-the-correct-target-framework"></a>2.選擇正確的**目標 Framework**

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

確定 [ **android 至目標**] 的最低設定為 **[Android 5.0 (棒糖)** ] 或更新版本: 

[![在 Visual Studio 中將目標 Framework 設定為 Android 5。0](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

確定 [目標 framework] 設定為 [ **Android 5.0 (棒糖)** ] 或更新版本:

[![在 Visual Studio for Mac 中將目標 Framework 設定為 Android 5。0](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

如需設定目標 framework 的詳細資訊, 請參閱[瞭解 ANDROID API 層級](~/android/app-fundamentals/android-api-levels.md)。


### <a name="3-edit-the-mainaxml-layout"></a>3.編輯**axml**版面配置

設定版面配置以包含`TextView`範例的`Button`和: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
  <ScrollView
    android:id="@+id/scroll"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#000000"
    android:fillViewport="true">
    <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:text="Main Activity"
        android:textSize="36sp"
        android:textColor="#006600" />
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:textColor="#cccccc"
        android:id="@+id/result" />
      <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="showNotification"
        android:text="Click Me!"
        android:id="@+id/click_button" />
    </LinearLayout>
  </ScrollView>
</FrameLayout>
```

### <a name="4-edit-the-mainactivitycs-source"></a>4.編輯**MainActivity.cs**來源

新增程式碼以遞增計數器, 並在每次按一下按鈕時顯示它: 

```csharp
[Activity (Label = "WearTest", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
  int count = 1;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    Button button = FindViewById<Button> (Resource.Id.click_button);
    TextView text = FindViewById<TextView> (Resource.Id.result);

    button.Click += delegate {
      text.Text = string.Format ("{0} clicks!", count++);
    };
  }
}
```

### <a name="5-setup-an-emulator-or-device"></a>5.設定模擬器或裝置

下一步是設定模擬器或裝置來部署和執行應用程式。 如果您還不熟悉部署和執行 Xamarin Android 應用程式的一般步驟, 請參閱[Hello, Android 快速入門](~/android/get-started/hello-android/hello-android-quickstart.md)。

如果您沒有 Android 磨損裝置 (例如 Android 磨損 Smartwatch), 您可以在模擬器上執行應用程式。 如需有關在模擬器上對磨損應用程式進行偵錯工具的詳細資訊, 請參閱[在模擬器上檢查 Android 磨損](~/android/wear/deploy-test/debug-on-emulator.md)。

如果您有 Android 磨損裝置 (例如 Android 磨損 Smartwatch), 您可以在裝置上執行應用程式, 而不是使用模擬器。 如需在磨損裝置上進行偵錯工具的詳細資訊, 請參閱[在磨損裝置上進行 Debug](~/android/wear/deploy-test/debug-on-device.md)。


### <a name="6-run-the-android-wear-app"></a>6.執行 Android 磨損應用程式

Android 磨損裝置應該會出現在裝置下拉式功能表中。 開始進行調試之前, 請務必選擇正確的 Android 磨損裝置或 AVD。 選取裝置之後, 請按一下 [播放] 按鈕, 將應用程式部署到模擬器或裝置。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![選擇 Visual Studio 裝置 功能表中的磨損 AVD](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![選擇 Visual Studio for Mac 裝置 功能表中的磨損 AVD](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

您可能會在一開始就看到一個 [**分鐘 ...** ] 訊息 (或一些其他插入式畫面): 

![監看模擬器只會顯示一分鐘 。](hello-wear-images/please-wait.png)

如果您使用監看式模擬器, 可能需要一段時間才能啟動應用程式。 當您使用藍牙時, 部署應用程式所需的時間會比透過 USB 更多。 (例如, 將此應用程式部署至 LG G Watch 大約需要5分鐘的時間, 即藍牙連線到第5部手機)。

應用程式成功部署之後, 磨損裝置的畫面應該會顯示如下的畫面:

[![磨損應用程式的初始畫面](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

請按 [ **CLICK ME!** ] 按鈕, 並在每次點擊時查看計數增量:

[![按三下滑鼠後磨損應用程式的螢幕擷取畫面](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>後續步驟

查看含有隨附電話應用程式的一些[磨損範例](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Android+wear), 包括 Android 磨損應用程式。

當您準備好散發應用程式時, 請參閱[使用封裝](~/android/wear/deploy-test/packaging.md)。


## <a name="related-links"></a>相關連結

- [按一下 [我的應用程式 (範例)]](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-weartest)
