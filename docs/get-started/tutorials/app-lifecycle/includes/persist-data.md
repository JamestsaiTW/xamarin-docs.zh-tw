---
ms.openlocfilehash: 16ceaba572ca932777bb366d9f7c58f6dcb24f70
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841418"
---
[`Application`](xref:Xamarin.Forms.Application) 子類別具有靜態 [`Properties`](xref:Xamarin.Forms.Application.Properties) 字典，可用來在生命週期狀態變更期間儲存資料。 此字典使用 `string` 索引鍵並儲存 `object` 值。 字典會自動儲存至裝置，並在應用程式重新啟動時重新填入。

> [!IMPORTANT]
> [`Properties`](xref:Xamarin.Forms.Application.Properties) 字典只可序列化基本類型以便儲存。

您將在此練習中修改應用程式，使其在背景執行時保存來自 [`Entry`](xref:Xamarin.Forms.Entry) 的文字，並在重新啟動應用程式時，將文字還原至 `Entry`。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 在 [方案總管]  中的 [AppLifecycleTutorial]  專案中，展開 **App.xaml**，然後按兩下 **App.xaml.cs** 將其開啟。 接著，在 **App.xaml.cs** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    此程式碼會定義 `DisplayText` 屬性及 `displayText` 常數。 當應用程式在背景執行或已終止時，`OnSleep` 方法覆寫會將 `DisplayText` 屬性值新增至 [`Properties`](xref:Xamarin.Forms.Application.Properties) 字典 (根據 `displayText` 的索引鍵)。 然後，當應用程式啟動時，如果 `Properties` 字典包含 `displayText` 索引鍵，索引鍵的值就會還原到 `DisplayText` 屬性。

    > [!IMPORTANT]
    > 請務必先檢查 [`Properties`](xref:Xamarin.Forms.Application.Properties) 字典是否有索引鍵，再進行存取，以避免發生意外的錯誤。

    在 `OnResume` 方法多載中，您不需要從 [`Properties`](xref:Xamarin.Forms.Application.Properties) 字典還原資料。 這是因為應用程式在背景執行時，應用程式和其狀態仍在記憶體中。

1. 在 [方案總管]  的 **AppLifecycleTutorial** 專案中，按兩下 **MainPage.xaml** 將其開啟。 然後在 **MainPage.xaml** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    此程式碼會以宣告的方式定義頁面的使用者介面，其包含 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 中的 [`Entry`](xref:Xamarin.Forms.Entry)。 [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) 屬性會指定 `Entry` 第一次顯示時呈現的預留位置文字，而名為 `OnEntryCompleted` 的事件處理常式會向 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 事件登記。 此外，`Entry` 具有以 `x:Name` 屬性指定的名稱。 這可讓程式碼後置檔案使用其獲派的名稱來存取 `Entry` 物件。

1. 在 [方案總管]  的 **AppLifecycleTutorial** 專案中展開 **MainPage.xaml**，然後按兩下 **MainPage.xaml.cs** 將其開啟。 然後，在 **MainPage.xaml.cs** 中，將 `OnAppearing` 方法的覆寫和 `OnEntryCompleted` 事件處理常式新增至類別：

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    `OnAppearing` 方法擷取 `App.DisplayText` 屬性的值，並將其設定為 [`Entry`](xref:Xamarin.Forms.Entry) 的 [`Text`](xref:Xamarin.Forms.Entry.Text) 屬性值。

    > [!NOTE]
    > `OnAppearing` 方法覆寫會在 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 配置完成後 (但在變成可見狀態之前) 執行。 因此，這是設定 Xamarin.Forms 檢視內容的好地方。

    以 return 鍵完成 [`Entry`](xref:Xamarin.Forms.Entry) 中的文字之後，`OnEntryCompleted` 會開始執行，而 `Entry` 文字會儲存在 `App.DisplayText` 屬性中。

1. 在 Visual Studio 工具列中，按下 [啟動]  按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選遠端 iOS 模擬器或 Android 模擬器內的應用程式。

    在 [`Entry`](xref:Xamarin.Forms.Entry) 中輸入一些文字並按下 return 鍵。 然後，點選 [首頁] 按鈕來叫用 `OnSleep` 方法，將應用程式放到背景。

    最後，再次從 Visual Studio 中啟動應用程式，先前輸入到 [`Entry`](xref:Xamarin.Forms.Entry) 中的文字將會還原：

    [![iOS 和 Android 中生命週期狀態變更期間其文字屬性已保留的輸入螢幕擷取畫面](../images/persist-data.png "生命週期狀態變更期間其文字屬性已保留的輸入")](../images/persist-data-large.png#lightbox "生命週期狀態變更期間其文字屬性已保留的輸入")

    如需有關將資料保存到屬性字典的詳細資訊，請參閱 [Xamarin.Forms 應用程式類別](~/xamarin-forms/app-fundamentals/application-class.md)指南中的[屬性字典](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary)。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 在 [Solution Pad]  中的 [AppLifecycleTutorial]  專案中，展開 **App.xaml**，然後按兩下 **App.xaml.cs** 將其開啟。 接著，在 **App.xaml.cs** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    此程式碼會定義 `DisplayText` 屬性及 `displayText` 常數。 當應用程式在背景執行或已終止時，`OnSleep` 方法覆寫會將 `DisplayText` 屬性值新增至 [`Properties`](xref:Xamarin.Forms.Application.Properties) 字典 (根據 `displayText` 的索引鍵)。 然後，當應用程式啟動時，如果 `Properties` 字典包含 `displayText` 索引鍵，索引鍵的值就會還原到 `DisplayText` 屬性。

    > [!IMPORTANT]
    > 請務必先檢查 [`Properties`](xref:Xamarin.Forms.Application.Properties) 字典是否有索引鍵，再進行存取，以避免發生意外的錯誤。

    在 `OnResume` 方法多載中，您不需要從 [`Properties`](xref:Xamarin.Forms.Application.Properties) 字典還原資料。 這是因為應用程式在背景執行時，應用程式和其狀態仍在記憶體中。

1. 在 [Solution Pad]  的 **AppLifecycleTutorial** 專案中，按兩下 **MainPage.xaml** 將其開啟。 然後在 **MainPage.xaml** 中，移除所有範本程式碼，並取代為下列程式碼：

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    此程式碼會以宣告的方式定義頁面的使用者介面，其包含 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 中的 [`Entry`](xref:Xamarin.Forms.Entry)。 [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) 屬性會指定 `Entry` 第一次顯示時呈現的預留位置文字，而名為 `OnEntryCompleted` 的事件處理常式會向 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 事件登記。 此外，`Entry` 具有以 `x:Name` 屬性指定的名稱。 這可讓程式碼後置檔案使用其獲派的名稱來存取 `Entry` 物件。

1. 在 [Solution Pad]  中的 [AppLifecycleTutorial]  專案中，展開 **MainPage.xaml**，然後按兩下 **MainPage.xaml.cs**將其開啟。 然後，在 **MainPage.xaml.cs** 中，將 `OnAppearing` 方法的覆寫和 `OnEntryCompleted` 事件處理常式新增至類別：

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    `OnAppearing` 方法擷取 `App.DisplayText` 屬性的值，並將其設定為 [`Entry`](xref:Xamarin.Forms.Entry) 的 [`Text`](xref:Xamarin.Forms.Entry.Text) 屬性值。

    > [!NOTE]
    > `OnAppearing` 方法覆寫會在 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 配置完成後 (但在變成可見狀態之前) 執行。 因此，這是設定 Xamarin.Forms 檢視內容的好地方。

    以 return 鍵完成 [`Entry`](xref:Xamarin.Forms.Entry) 中的文字之後，`OnEntryCompleted` 會開始執行，而 `Entry` 文字會儲存在 `App.DisplayText` 屬性中。

1. 在 Visual Studio for Mac 工具列中，按下 [啟動]  按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選 iOS 模擬器或 Android 模擬器內的應用程式。

    在 [`Entry`](xref:Xamarin.Forms.Entry) 中輸入一些文字並按下 return 鍵。 然後，點選 [首頁] 按鈕來叫用 `OnSleep` 方法，將應用程式放到背景。

    最後，再次從 Visual Studio for Mac 中啟動應用程式，先前輸入到 [`Entry`](xref:Xamarin.Forms.Entry) 中的文字將會還原：

    [![iOS 和 Android 中生命週期狀態變更期間其文字屬性已保留的輸入螢幕擷取畫面](../images/persist-data.png "生命週期狀態變更期間其文字屬性已保留的輸入")](../images/persist-data-large.png#lightbox "生命週期狀態變更期間其文字屬性已保留的輸入")

    如需有關將資料保存到屬性字典的詳細資訊，請參閱 [Xamarin.Forms 應用程式類別](~/xamarin-forms/app-fundamentals/application-class.md)指南中的[屬性字典](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary)。
