---
title: 建立單頁 Xamarin.Forms 應用程式
description: 本文說明如何建立單一頁面跨平臺 Xamarin 應用程式, 這可讓您輸入附注並將它保存在裝置儲存體中。
zone_pivot_groups: platform-dev16
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 48af81856ebe92a712b5d41d8d856bb00a526982
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655001"
---
# <a name="create-a-single-page-xamarinforms-application"></a>建立單一頁面 Xamarin. Forms 應用程式

[![下載範例](~/media/shared/download.png) 下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)

在本快速入門中, 您將瞭解如何:

- 建立跨平臺的 Xamarin Forms 應用程式。
- 使用可延伸應用程式標記語言 (XAML) 來定義頁面的使用者介面。
- 從程式碼與 XAML 使用者介面元素互動。

本快速入門會逐步解說如何建立跨平臺的 Xamarin Forms 應用程式, 讓您輸入附注, 並將它保存到裝置存放裝置。 最終的應用程式如下所示：

[![](single-page-images/screenshots-sml.png "Notes 應用程式")](single-page-images/screenshots.png#lightbox "Notes 應用程式")

::: zone pivot="windows"

### <a name="prerequisites"></a>必要條件

- Visual Studio 2019 (最新版本), 並已安裝**具有 .net**工作負載的行動裝置開發。
- 的C#知識。
- 選擇性在 iOS 上建立應用程式的配對 Mac。

如需這些必要條件的詳細資訊, 請參閱[安裝 Xamarin](~/get-started/installation/index.md)。 如需有關將 Visual Studio 2019 連線至 Mac 建置主機的相關資訊，請參閱[為 Xamarin.iOS 開發與 Mac 配對](~/ios/get-started/installation/windows/connecting-to-mac/index.md)。

## <a name="get-started-with-visual-studio-2019"></a>開始使用 Visual Studio 2019

1. 啟動 Visual Studio 2019, 然後在 [開始] 視窗中, 按一下 [**建立新專案**] 來建立新的專案:

    ![](single-page-images/vs/new-solution-2019.png "新增專案")

2. 在 [**建立新專案**] 視窗的 [ **專案類型**] 下拉式選單中, 選取 [行動**應用程式 (Xamarin** ] 範本), 然後按 [**下一步]** 按鈕:

    ![](single-page-images/vs/new-project-2019.png "跨平台專案範本")

3. 在 [設定**您的新專案**] 視窗中, 將 [**專案名稱**] 設為 [**附注**], 選擇專案的適當位置, 然後按一下 [**建立**] 按鈕:

    ![](single-page-images/vs/configure-project.png "設定您的專案")

    > [!IMPORTANT]
    > 本快速入門中的 C# 和 XAML 程式碼片段，要求將解決方案命名為 **Notes**。 當您從本快速入門將程式碼複製到解決方案時，使用不同的名稱會導致建置錯誤。

4. 在 [**新增跨平臺應用程式**] 對話方塊中, 按一下 [**空白應用程式**], 然後按一下 [**確定]** 按鈕:

    ![](single-page-images/vs/new-app-2019.png "新增跨平台應用程式")

    如需有關所建立 .NET Standard 程式庫的詳細資訊，請參閱 [Xamarin.Forms 快速入門深度剖析](deepdive.md)中的 [Xamarin.Forms 應用程式的結構](deepdive.md#anatomy-of-a-xamarinforms-application)。

5. 在 [方案總管]  的 **Notes** 專案中，按兩下 **MainPage.xaml** 將其開啟：

    ![](single-page-images/vs/open-mainpage-xaml-2019.png "開啟 MainPage.xaml")

6. 在 **MainPage.xaml** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    此程式碼會以宣告的方式定義頁面的使用者介面, 其中[`Label`](xref:Xamarin.Forms.Label)包含要顯示文字的[`Editor`](xref:Xamarin.Forms.Editor) 、文字輸入的, 以及[`Button`](xref:Xamarin.Forms.Button)指示應用程式儲存或刪除檔案的兩個實例。 兩個 `Button` 執行個體以水平方式配置在 [`Grid`](xref:Xamarin.Forms.Grid) 中，而 `Label`、`Editor` 和 `Grid` 以垂直方式配置在 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 中。 如需建立使用者介面的詳細資訊, 請參閱 Xamarin 中的[使用者介面](deepdive.md#user-interface)[快速入門深入探討](deepdive.md)。

    按下 **CTRL+S** 以將變更儲存到 **MainPage.xaml**，然後關閉檔案。

7. 在 [方案總管]  的 **Notes** 專案中展開 **MainPage.xaml**，然後按兩下 **MainPage.xaml.cs** 將其開啟：

    ![](single-page-images/vs/open-mainpage-codebehind-2019.png "開啟 MainPage.xaml.cs")

8. 在 **MainPage.xaml.cs** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    此程式碼定義了 `_fileName` 欄位，這個欄位會參考名為 `notes.txt` 的檔案，以將附註資料儲存在應用程式的本機應用程式資料夾中。 執行頁面建構函式時會讀取檔案 (如果存在的話)，並將其顯示在[`Editor`](xref:Xamarin.Forms.Editor)中。 當按下 [儲存]  [`Button`](xref:Xamarin.Forms.Button)時，即會執行 `OnSaveButtonClicked` 事件處理常式，這會將`Editor`的內容儲存到檔案。 當按下 [刪除]  `Button`時，即會執行 `OnDeleteButtonClicked` 事件處理常式，這會刪除檔案 (假設檔案存在)，並從`Editor`中移除任何文字。 如需使用者互動的詳細資訊, 請參閱在 Xamarin 中[回應使用者互動](deepdive.md#responding-to-user-interaction)[快速深入探討](deepdive.md)。

    按下 **CTRL+S** 以將變更儲存到 **MainPage.xaml.cs**，然後關閉檔案。

### <a name="building-the-quickstart"></a>建置快速入門

1. 在 Visual Studio 中，選取 [建置] > [建置方案]  功能表項目 (或按下 F6)。 系統將建置解決方案，且 Visual Studio 狀態列中將會出現成功訊息：

      ![](single-page-images/vs/build-succeeded.png "組建成功")

    如果發生錯誤，請重複上述步驟並更正任何錯誤，直到解決方案建置成功為止。

2. 在 Visual Studio 工具列中，按下 [啟動]  按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選擇 Android Emulator 中的應用程式：

    ![](single-page-images/vs/android-start.png "Visual Studio Android 工具列")

    [![](single-page-images/vs/notes-android.png "Android Emulator 中的附注")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    輸入附註，然後按 [儲存]  按鈕。

    如需有關如何在每個平臺上啟動應用程式的詳細資訊, 請參閱在 Xamarin 中的[每個平臺上啟動應用程式](deepdive.md#launching-the-application-on-each-platform) [。表單快速入門深入探討](deepdive.md)。

    > [!NOTE]
    > 只有在您[配對的 Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 符合 Xamarin.Forms 開發的系統需求時，才應該執行下列步驟。

3. 在 Visual Studio 工具列中，以滑鼠右鍵按一下 **Notes.iOS** 專案，然後選取 [設定為啟始專案]  。

      ![](single-page-images/vs/set-as-startup-project-ios.png "將 iOS 設定為啟始專案")

4. 在 Visual Studio 工具列中，按下 [啟動]  按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選擇 [iOS 遠端模擬器](~/tools/ios-simulator/index.md)中的應用程式：

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS 工具列")

    [IOS 模擬器中的附注![(single-page-images/vs/notes-ios.png "")] ](single-page-images/vs/notes-ios-large.png#lightbox "IOS 模擬器中的附注")

    輸入附註，然後按 [儲存]  按鈕。

    如需有關如何在每個平臺上啟動應用程式的詳細資訊, 請參閱在 Xamarin 中的[每個平臺上啟動應用程式](deepdive.md#launching-the-application-on-each-platform) [。表單快速入門深入探討](deepdive.md)。

::: zone-end
::: zone pivot="win-vs2017"

### <a name="prerequisites"></a>必要條件

- Visual Studio 2017, 已安裝**具有 .net**工作負載的行動裝置開發。
- 的C#知識。
- 選擇性在 iOS 上建立應用程式的配對 Mac。

如需這些必要條件的詳細資訊, 請參閱[安裝 Xamarin](~/get-started/installation/index.md)。 如需有關將 Visual Studio 2019 連線至 Mac 建置主機的相關資訊，請參閱[為 Xamarin.iOS 開發與 Mac 配對](~/ios/get-started/installation/windows/connecting-to-mac/index.md)。

## <a name="get-started-with-visual-studio-2017"></a>開始使用 Visual Studio 2017

1. 啟動 Visual Studio 2017, 然後在 [開始] 頁面上, 按一下 [**建立新專案**] 以建立新的專案:

    ![](single-page-images/vs/new-solution.png "新增專案")

2. 在 [新增專案]  對話方塊中，按一下 [跨平台]  、選取 [行動應用程式 (Xamarin.Forms)]  範本、將 [名稱] 設定為 **Notes**、選擇專案的適當位置，然後按一下 [確定]  按鈕：

    ![](single-page-images/vs/new-project.png "跨平台專案範本")

    > [!IMPORTANT]
    > 本快速入門中的 C# 和 XAML 程式碼片段，要求將解決方案命名為 **Notes**。 當您從本快速入門將程式碼複製到解決方案時，使用不同的名稱會導致建置錯誤。

3. 在 [新增跨平台應用程式]  對話方塊中，按一下 [空白應用程式]  ，並選取 [.NET Standard]  作為 [程式碼共用策略]，然後按一下 [確定]  按鈕：

    ![](single-page-images/vs/new-app.png "新增跨平台應用程式")

    如需有關所建立 .NET Standard 程式庫的詳細資訊，請參閱 [Xamarin.Forms 快速入門深度剖析](deepdive.md)中的 [Xamarin.Forms 應用程式的結構](deepdive.md#anatomy-of-a-xamarinforms-application)。

4. 在 [方案總管]  的 **Notes** 專案中，按兩下 **MainPage.xaml** 將其開啟：

    ![](single-page-images/vs/open-mainpage-xaml.png "開啟 MainPage.xaml")

5. 在 **MainPage.xaml** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    此程式碼會以宣告的方式定義頁面的使用者介面, 其中[`Label`](xref:Xamarin.Forms.Label)包含要顯示文字的[`Editor`](xref:Xamarin.Forms.Editor) 、文字輸入的, 以及[`Button`](xref:Xamarin.Forms.Button)指示應用程式儲存或刪除檔案的兩個實例。 兩個 `Button` 執行個體以水平方式配置在 [`Grid`](xref:Xamarin.Forms.Grid) 中，而 `Label`、`Editor` 和 `Grid` 以垂直方式配置在 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 中。 如需建立使用者介面的詳細資訊, 請參閱 Xamarin 中的[使用者介面](deepdive.md#user-interface)[快速入門深入探討](deepdive.md)。

    按下 **CTRL+S** 以將變更儲存到 **MainPage.xaml**，然後關閉檔案。

6. 在 [方案總管]  的 **Notes** 專案中展開 **MainPage.xaml**，然後按兩下 **MainPage.xaml.cs** 將其開啟：

    ![](single-page-images/vs/open-mainpage-codebehind.png "開啟 MainPage.xaml.cs")

7. 在 **MainPage.xaml.cs** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    此程式碼定義了 `_fileName` 欄位，這個欄位會參考名為 `notes.txt` 的檔案，以將附註資料儲存在應用程式的本機應用程式資料夾中。 執行頁面建構函式時會讀取檔案 (如果存在的話)，並將其顯示在[`Editor`](xref:Xamarin.Forms.Editor)中。 當按下 [儲存]  [`Button`](xref:Xamarin.Forms.Button)時，即會執行 `OnSaveButtonClicked` 事件處理常式，這會將`Editor`的內容儲存到檔案。 當按下 [刪除]  `Button`時，即會執行 `OnDeleteButtonClicked` 事件處理常式，這會刪除檔案 (假設檔案存在)，並從`Editor`中移除任何文字。 如需使用者互動的詳細資訊, 請參閱在 Xamarin 中[回應使用者互動](deepdive.md#responding-to-user-interaction)[快速深入探討](deepdive.md)。

    按下 **CTRL+S** 以將變更儲存到 **MainPage.xaml.cs**，然後關閉檔案。

### <a name="building-the-quickstart"></a>建置快速入門

1. 在 Visual Studio 中，選取 [建置] > [建置方案]  功能表項目 (或按下 F6)。 系統將建置解決方案，且 Visual Studio 狀態列中將會出現成功訊息：

      ![](single-page-images/vs/build-succeeded.png "組建成功")

    如果發生錯誤，請重複上述步驟並更正任何錯誤，直到解決方案建置成功為止。

2. 在 Visual Studio 工具列中，按下 [啟動]  按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選擇 Android Emulator 中的應用程式：

    ![](single-page-images/vs/android-start.png "Visual Studio Android 工具列")

    [![](single-page-images/vs/notes-android.png "Android Emulator 中的附注")](single-page-images/vs/notes-android-large.png#lightbox "Notes in the Android Simulator")

    輸入附註，然後按 [儲存]  按鈕。

    如需有關如何在每個平臺上啟動應用程式的詳細資訊, 請參閱在 Xamarin 中的[每個平臺上啟動應用程式](deepdive.md#launching-the-application-on-each-platform) [。表單快速入門深入探討](deepdive.md)。

    > [!NOTE]
    > 只有在您[配對的 Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 符合 Xamarin.Forms 開發的系統需求時，才應該執行下列步驟。

3. 在 Visual Studio 工具列中，以滑鼠右鍵按一下 **Notes.iOS** 專案，然後選取 [設定為啟始專案]  。

      ![](single-page-images/vs/set-as-startup-project-ios.png "將 iOS 設定為啟始專案")

4. 在 Visual Studio 工具列中，按下 [啟動]  按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選擇 [iOS 遠端模擬器](~/tools/ios-simulator/index.md)中的應用程式：

    ![](single-page-images/vs/ios-start.png "Visual Studio iOS 工具列")

    [IOS 模擬器中的附注![(single-page-images/vs/notes-ios.png "")] ](single-page-images/vs/notes-ios-large.png#lightbox "IOS 模擬器中的附注")

    輸入附註，然後按 [儲存]  按鈕。

    如需有關如何在每個平臺上啟動應用程式的詳細資訊, 請參閱在 Xamarin 中的[每個平臺上啟動應用程式](deepdive.md#launching-the-application-on-each-platform) [。表單快速入門深入探討](deepdive.md)。

::: zone-end
::: zone pivot="macos"

### <a name="prerequisites"></a>必要條件

- Visual Studio for Mac (最新版本), 並已安裝 iOS 和 Android 平臺支援。
- Xcode (最新版本)。
- 的C#知識。

如需這些必要條件的詳細資訊, 請參閱[安裝 Xamarin](~/get-started/installation/index.md)。

## <a name="get-started-with-visual-studio-for-mac"></a>Visual Studio for Mac 使用者入門

1. 啟動 Visual Studio for Mac, 然後在 [開始] 視窗中按一下 [**新增**], 以建立新的專案:

    ![](single-page-images/vsmac/new-project.png "新增方案")

2. 在 [為新專案選擇範本]  對話方塊中，按一下 [多平台] > [應用程式]  、選取 [空白的 Forms App]  範本，然後按 [下一步]  按鈕：

    ![](single-page-images/vsmac/choose-template.png "選擇範本")

3. 在 [設定空白的表單應用程式]  對話方塊中，將新的應用程式命名為 **Notes**、確保已選取 [使用 .NET Standard]  選項按鈕，然後按一下 [下一步]  按鈕：    

    ![](single-page-images/vsmac/configure-app.png "設定表單應用程式")

4. 在 [設定新的空白表單應用程式]  對話方塊中，將 [方案名稱] 和 [專案名稱] 設定為 **Notes**、選擇專案的合適位置，然後按一下 [建立]  按鈕以建立專案：

    ![](single-page-images/vsmac/configure-project.png "設定表單專案")

    > [!IMPORTANT]
    > 本快速入門中的 C# 和 XAML 程式碼片段要求將解決方案和專案全都命名為 **Notes**。 當您從本快速入門將程式碼複製到專案時，使用不同的名稱會導致建置錯誤。

    如需有關所建立 .NET Standard 程式庫的詳細資訊，請參閱 [Xamarin.Forms 快速入門深度剖析](deepdive.md)中的 [Xamarin.Forms 應用程式的結構](deepdive.md#anatomy-of-a-xamarinforms-application)。

5. 在 [Solution Pad]  的 **Notes** 專案中，按兩下 **MainPage.xaml** 將其開啟：

    ![](single-page-images/vsmac/mainpage-xaml.png "MainPage.xaml")

6. 在 **MainPage.xaml** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    此程式碼會以宣告的方式定義頁面的使用者介面, 其中[`Label`](xref:Xamarin.Forms.Label)包含要顯示文字的[`Editor`](xref:Xamarin.Forms.Editor) 、文字輸入的, 以及[`Button`](xref:Xamarin.Forms.Button)指示應用程式儲存或刪除檔案的兩個實例。 兩個 `Button` 執行個體以水平方式配置在 [`Grid`](xref:Xamarin.Forms.Grid) 中，而 `Label`、`Editor` 和 `Grid` 以垂直方式配置在 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 中。 如需建立使用者介面的詳細資訊, 請參閱 Xamarin 中的[使用者介面](deepdive.md#user-interface)[快速入門深入探討](deepdive.md)。

    選擇 [檔案] > [儲存]  (或按下 **&#8984; + S**) 以將變更儲存到 **MainPage.xaml**，然後關閉檔案。

7. 在 [Solution Pad]  的 **Notes** 專案中展開 **MainPage.xaml**，然後按兩下 **MainPage.xaml.cs** 將其開啟：

    ![](single-page-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

8. 在 **MainPage.xaml.cs** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                editor.Text = string.Empty;
            }
        }
    }
    ```

    此程式碼定義了 `_fileName` 欄位，這個欄位會參考名為 `notes.txt` 的檔案，以將附註資料儲存在應用程式的本機應用程式資料夾中。 執行頁面建構函式時會讀取檔案 (如果存在的話)，並將其顯示在[`Editor`](xref:Xamarin.Forms.Editor)中。 當按下 [儲存]  [`Button`](xref:Xamarin.Forms.Button)時，即會執行 `OnSaveButtonClicked` 事件處理常式，這會將`Editor`的內容儲存到檔案。 當按下 [刪除]  `Button`時，即會執行 `OnDeleteButtonClicked` 事件處理常式，這會刪除檔案 (假設檔案存在)，並從`Editor`中移除任何文字。 如需使用者互動的詳細資訊, 請參閱在 Xamarin 中[回應使用者互動](deepdive.md#responding-to-user-interaction)[快速深入探討](deepdive.md)。

    選擇 [檔案] > [儲存]  (或按下 **&#8984; + S**) 以將變更儲存到 **MainPage.xaml.cs**，然後關閉檔案。

### <a name="building-the-quickstart"></a>建置快速入門

1. 在 Visual Studio for Mac 中，選取 [建置] > [全部建置]  功能表項目 (或按下 **&#8984; + B**)。 系統將建置專案，且 Visual Studio for Mac 工具列中將會出現成功訊息。

      ![](single-page-images/vsmac/build-successful.png "建置成功")

    如果發生錯誤，請重複上述步驟並更正任何錯誤，直到專案建置成功為止。

2. 在  **Solution Pad**中, 選取 **記事** 專案, 按一下滑鼠右鍵, 然後選取 **設定為啟始專案**:

      ![](single-page-images/vsmac/set-startup-project-ios.png "將 iOS 設定為啟始專案")

3. 在 Visual Studio for Mac 工具列中，按下 [啟動]  按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選擇 iOS 模擬器內的應用程式：

      ![](single-page-images/vsmac/start.png "Visual Studio for Mac 工具列")

      [IOS 模擬器中的附注![(single-page-images/vsmac/notes-ios.png "")] ](single-page-images/vsmac/notes-ios-large.png#lightbox "IOS 模擬器中的附注")

    輸入附註，然後按 [儲存]  按鈕。

    如需有關如何在每個平臺上啟動應用程式的詳細資訊, 請參閱在 Xamarin 中的[每個平臺上啟動應用程式](deepdive.md#launching-the-application-on-each-platform) [。表單快速入門深入探討](deepdive.md)。

4. 在  **Solution Pad**中, 選取**Droid**專案、按一下滑鼠右鍵, 然後選取 **設定為啟始專案**:

      ![](single-page-images/vsmac/set-startup-project-android.png "將 Android 設定為啟始專案")

5. 在 Visual Studio for Mac 工具列中，按下 [啟動]  按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選擇 Android Emulator 內的應用程式：

      [![](single-page-images/vsmac/notes-android.png "Android Emulator 中的附注")](single-page-images/vsmac/notes-android-large.png#lightbox "Notes in the Android Simulator")

    輸入附註，然後按 [儲存]  按鈕。

    如需有關如何在每個平臺上啟動應用程式的詳細資訊, 請參閱在 Xamarin 中的[每個平臺上啟動應用程式](deepdive.md#launching-the-application-on-each-platform) [。表單快速入門深入探討](deepdive.md)。

::: zone-end

## <a name="next-steps"></a>後續步驟

在本快速入門中, 您已瞭解如何:

- 建立跨平臺的 Xamarin Forms 應用程式。
- 使用可延伸應用程式標記語言 (XAML) 來定義頁面的使用者介面。
- 從程式碼與 XAML 使用者介面元素互動。

若要將此單一頁面應用程式轉換成多頁應用程式, 請繼續進行下一個快速入門。

> [!div class="nextstepaction"]
> [下一個](multi-page.md)

## <a name="related-links"></a>相關連結

- [Notes (範例)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)
- [Xamarin. 表單快速入門深入探討](deepdive.md)
