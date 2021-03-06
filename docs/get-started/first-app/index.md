---
title: 建置您的第一個 Xamarin.Forms 應用程式
description: 示範如何在 Visual Studio 中建置第一個 Xamarin.Forms 應用程式的影片指南。
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468
ms.technology: xamarin-forms
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 05/23/2019
ms.openlocfilehash: 245b41eea556ef36c81b337b57ce58d922e4e8fd
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653674"
---
# <a name="build-your-first-xamarinforms-app"></a>建置您的第一個 Xamarin.Forms 應用程式

觀看這段影片，並遵循指示建立第一個使用 Xamarin.Forms 的行動應用程式。 

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-Android-App-with-Visual-Studio-2019-and-Xamarin/player]

## <a name="step-by-step-instructions-for-windows"></a>適用於 Windows 的逐步指示

[![下載範例](~/media/shared/download.png) 下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

請遵循下列步驟及上面的影片進行：

1. 選擇 [檔案] **> [新增 > 專案**...] 或按 [**建立新專案 ...** ] 按鈕:

    [![建立新專案](images/win-2019/01-sml.png)](images/win-2019/01.png#lightbox)

2. 搜尋「Xamarin」, 或從 [**專案類型**] 功能表選擇 [ **Mobile** ]。 選取 [行動**應用程式 (Xamarin)** ] 專案類型:

    [![Xamarin 專案的篩選](images/win-2019/02-sml.png)](images/win-2019/02.png#lightbox)

3. 選擇專案名稱&ndash;此範例使用 "AwesomeApp":

    [![選擇專案名稱](images/win-2019/03-sml.png)](images/win-2019/03.png#lightbox)

4. 按一下 [**空白**] 專案類型, 並確定已選取 [ **Android** ] 和 [ **iOS** ]:

    [![[Android] 和 [iOS] 搭配 [.NET Standard]](images/win-2019/04-sml.png)](images/win-2019/04.png#lightbox)

5. 等候 NuGet 套件還原完成 (狀態列中將會顯示 [還原完成] 訊息)。

6. 按下偵錯按鈕 (或 [偵錯] > [開始偵錯]  功能表項目) 來啟動 Android Emulator。

7. 編輯 **MainPage.xaml**，並在 `</StackLayout>` 結尾前面新增此 XAML：

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

8. 編輯 **MainPage.xaml.cs**，並將這段程式碼新增至類別結尾：

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

9. 偵錯 Android 上的應用程式：

    ![Android 應用程式](images/win/07-sml.png)

    > [!TIP]
    > 您可以使用網路相連的 Mac 電腦，從 Visual Studio 建置及偵錯 iOS 應用程式。 如需詳細資訊，請參閱[安裝指示](~/ios/get-started/installation/windows/index.md)。

## <a name="build-an-ios-app-in-visual-studio-2019"></a>在 Visual Studio 2019 中建立 iOS 應用程式

這段影片涵蓋使用 Windows 上的 Visual Studio 2019 建立和測試 iOS 應用程式的流程:

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-iOS-App-with-Visual-Studio-2019-and-Xamarin/player]

::: zone-end
::: zone pivot="win-vs2017"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>適用於 Windows 的逐步指示

[![下載範例](~/media/shared/download.png) 下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

請遵循下列步驟及上面的影片進行：

1. 選擇 [檔案] > [新增] > [專案...]  或按下 [建立新專案...]  按鈕，然後選取 [Visual C#] > [跨平台] > [行動應用程式 (Xamarin.Forms)]  ：

    [![行動應用程式 (Xamarin.Forms)](images/win/01-sml.png)](images/win/01.png#lightbox)

2. 確定已選取 [Android]  和 [iOS]  ，並搭配 [.NET Standard]  程式碼共用：

    [![[Android] 和 [iOS] 搭配 [.NET Standard]](images/win/02-sml.png)](images/win/02.png#lightbox)

3. 等候 NuGet 套件還原完成 (狀態列中將會顯示 [還原完成] 訊息)。

4. 按下偵錯按鈕 (或 [偵錯] > [開始偵錯]  功能表項目) 來啟動 Android Emulator。

5. 編輯 **MainPage.xaml**，並在 `</StackLayout>` 結尾前面新增此 XAML：

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

6. 編輯 **MainPage.xaml.cs**，並將這段程式碼新增至類別結尾：

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. 偵錯 Android 上的應用程式：

    ![Android 應用程式](images/win/07-sml.png)

    > [!TIP]
    > 您可以使用網路相連的 Mac 電腦，從 Visual Studio 建置及偵錯 iOS 應用程式。 如需詳細資訊，請參閱[安裝指示](~/ios/get-started/installation/windows/index.md)。

::: zone-end
::: zone pivot="macos"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-iOS--Android-App-in-Visual-Studio-for-Mac/player]

## <a name="step-by-step-instructions-for-mac"></a>適用於 Mac 的逐步指示

[![下載範例](~/media/shared/download.png) 下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

請遵循下列步驟及上面的影片進行：

1. 選擇 [檔案] > [新增方案...]  或按下 [新增專案...]  按鈕，然後選取 [多平台] > [應用程式] > [空白的 Forms App]  ：

    [![空白的 Forms App](images/01-sml.png)](images/01.png#lightbox)

2. 確定已選取 [Android]  和 [iOS]  ，並搭配 [.NET Standard]  程式碼共用：

    [![[Android] 和 [iOS] 搭配 [.NET Standard]](images/02-sml.png)](images/02.png#lightbox)

3. 以滑鼠右鍵按一下方案來還原 NuGet 套件：

    ![Android 應用程式](images/03-sml.png)

4. 按下偵錯按鈕 (或 [執行] > [開始偵錯]  ) 來啟動 Android Emulator。

5. 編輯 **MainPage.xaml**，並在 `</StackLayout>` 結尾前面新增此 XAML：

    ```xaml
    <Button Text="Click Me" Clicked="Handle_Clicked" />
    ```

6. 編輯 **MainPage.xaml.cs**，並將這段程式碼新增至類別結尾：

    ```csharp
    int count = 0;
    void Handle_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. 偵錯 Android 上的應用程式：

    ![Android 應用程式](images/07-sml.png)

8. 按一下滑鼠右鍵將 iOS 設定為 [啟始專案]  ：

    [![將啟始專案設定為 iOS](images/08-sml.png)](images/08.png#lightbox)

9. 偵錯 iOS 上的應用程式：

    ![iOS 應用程式](images/09-sml.png)

::: zone-end

您可以從[範例庫](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)下載完整的程式碼，或在 [GitHub](https://github.com/xamarin/xamarin-forms-samples/tree/master/GetStarted/FirstApp) 上進行檢視。

## <a name="next-steps"></a>後續步驟

- [單一頁面快速入門](~/get-started/quickstarts/single-page.md)&ndash;建立功能更強大的應用程式。
- [Xamarin.Forms 範例](~/xamarin-forms/samples/index.yml) &ndash; 下載並執行程式碼範例和範例應用程式。
- [建立行動應用程式電子書](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; 教導 Xamarin.Forms 開發的深入章節，提供 PDF 並包含數以百計的其他範例。
