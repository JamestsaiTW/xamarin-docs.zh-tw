
Xamarin.Forms 具有強制回應快顯視窗 (也稱為動作表)，可用來引導使用者執行工作。 在本練習中，您會使用 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 方法 (來自 [`Page`](xref:Xamarin.Forms.Page) 類別) 顯示可引導使用者執行工作的動作表。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 在 **MainPage.xaml** 中，新增可顯示動作表的新 [`Button`](xref:Xamarin.Forms.Button) 宣告：

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

     [`Button.Text`](xref:Xamarin.Forms.Button.Text) 屬性可指定出現在 `Button` 中的文字。 此外，[`Clicked`](xref:Xamarin.Forms.Button.Clicked) 事件會設定為名為 `OnDisplayActionSheetButtonClicked` 的事件處理常式 (將在下一個步驟中建立)。

1. 在 [方案總管] 的 **PopupsTutorial** 專案中展開 **MainPage.xaml**，然後按兩下 **MainPage.xaml.cs** 將其開啟。 然後，在 **MainPage.xaml.cs** 中，將 `OnDisplayActionSheetButtonClicked` 事件處理常式新增至類別：

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    `OnDisplayActionSheetButtonClicked`方法會在點選 [`Button`](xref:Xamarin.Forms.Button) 時執行。 這個方法會呼叫 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 方法，向使用者呈現如何繼續執行工作的一組替代項目。 使用者選取其中一個替代項目後，系統便會以 `string` 傳回選取項目。

    > [!IMPORTANT]
    > [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 是非同步的方法，因此請一律使用 `await` 關鍵字來等候。

1. 在 Visual Studio 工具列中，按下 [啟動] 按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選遠端 iOS 模擬器或 Android 模擬器內的應用程式。 然後，點選您新增至 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 的 [`Button`](xref:Xamarin.Forms.Button)：

    [![iOS 和 Android 上動作表的螢幕擷取畫面](../images/actionsheet.png "引導使用者執行工作的動作表")](../images/actionsheet-large.png#lightbox "引導使用者執行工作的動作表")

    請觀察一下，在選取動作表對話方塊中的替代項目之後，選取項目便會輸出到 Visual Studio 的 [輸出] 視窗。

    如需如何顯示動作表的詳細資訊，請參閱[顯示快顯視窗](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md)指南中的[引導使用者執行工作](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md#guiding-users-through-tasks)。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 在 **MainPage.xaml** 中，新增可顯示動作表的新 [`Button`](xref:Xamarin.Forms.Button) 宣告：

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

    [`Button.Text`](xref:Xamarin.Forms.Button.Text) 屬性可指定出現在 `Button` 中的文字。 此外，[`Clicked`](xref:Xamarin.Forms.Button.Clicked) 事件會設定為名為 `OnDisplayActionSheetButtonClicked` 的事件處理常式 (將在下一個步驟中建立)。

1. 在 [Solution Pad] 的 **PopupsTutorial** 專案中展開 **MainPage.xaml**，然後按兩下 **MainPage.xaml.cs** 將其開啟。 然後，在 **MainPage.xaml.cs** 中，將 `OnDisplayActionSheetButtonClicked` 事件處理常式新增至類別：

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    `OnDisplayActionSheetButtonClicked`方法會在點選 [`Button`](xref:Xamarin.Forms.Button) 時執行。 這個方法會呼叫 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 方法，向使用者呈現如何繼續執行工作的一組替代項目。 使用者選取其中一個替代項目後，系統便會以 `string` 傳回選取項目。

    > [!IMPORTANT]
    > [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 是非同步的方法，因此請一律使用 `await` 關鍵字來等候。

1. 在 Visual Studio for Mac 工具列中，按下 [啟動] 按鈕 (類似於 [播放] 按鈕的三角形按鈕)，以啟動所選 iOS 模擬器或 Android 模擬器內的應用程式。 然後，點選您新增至 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 的 [`Button`](xref:Xamarin.Forms.Button)：

    [![iOS 和 Android 上動作表的螢幕擷取畫面](../images/actionsheet.png "引導使用者執行工作的動作表")](../images/actionsheet-large.png#lightbox "引導使用者執行工作的動作表")

    請觀察一下，在選取動作表對話方塊中的替代項目之後，選取項目便會輸出到 Visual Studio for Mac 的 [輸出] 視窗。

    如需如何顯示動作表的詳細資訊，請參閱[顯示快顯視窗](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md)指南中的[引導使用者執行工作](~/xamarin-forms/app-fundamentals/navigation/pop-ups.md#guiding-users-through-tasks)。