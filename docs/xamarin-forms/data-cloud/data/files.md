---
title: Xamarin.Forms 中的檔案處理
description: 使用 .NET Standard 程式庫的程式碼或使用內嵌的資源，可以使用 Xamarin.Forms 處理檔案。
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 0d4e32b7bf98758f12dc038e0b61ffa0132f234d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529234"
---
# <a name="file-handling-in-xamarinforms"></a>Xamarin.Forms 中的檔案處理

[![下載範例](~/media/shared/download.png)下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)

_使用 .NET Standard 程式庫的程式碼或使用內嵌的資源，可以使用 Xamarin.Forms 處理檔案。_

## <a name="overview"></a>總覽

Xamarin.Forms 程式碼會在多個平台上執行 - 每一個都有自己的檔案系統。 在過去，這表示在每個平台上使用原生檔案 API 最容易執行讀取和寫入檔案。 或者，內嵌的資源是更簡單的散發資料檔案與應用程式解決方案。 不過，使用 .NET Standard 2.0 就可以在 .NET Standard 程式庫中共用檔案存取碼。

如需處理影像檔的相關資訊，請參閱[處理影像](~/xamarin-forms/user-interface/images.md)頁面。

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>儲存及載入檔案

`System.IO` 類別可用來存取每個平台上的檔案系統。 `File` 類別可讓您建立、刪除和讀取檔案，而 `Directory` 類別可讓您建立、刪除或列舉目錄內容。 您也可以使用 `Stream` 子類別，其可提供更高的檔案作業控制權 (例如壓縮或在檔案中搜尋位置)。

您可以使用 `File.WriteAllText` 方法撰寫文字檔：

```csharp
File.WriteAllText(fileName, text);
```

您可以使用 `File.ReadAllText` 方法讀取文字檔：

```csharp
string text = File.ReadAllText(fileName);
```

此外，`File.Exists` 方法可以判斷指定的檔案是否存在：

```csharp
bool doesExist = File.Exists(fileName);
```

使用 [`Environment.SpecialFolder`](xref:System.Environment.SpecialFolder) 列舉的值作為 `Environment.GetFolderPath` 方法的第一個引數，就可以從 .NET Standard 程式庫判斷每個平台的檔案路徑。 然後使用 `Path.Combine` 方法與檔案名稱相結合：

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

這些作業會在範例應用程式中示範，包括儲存及載入文字的頁面：

[![儲存及載入文字](files-images/saveandload-sml.png "在應用程式中儲存及載入檔案")](files-images/saveandload.png#lightbox "在應用程式中儲存及載入檔案")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>載入內嵌為資源的檔案

若要將檔案內嵌至 **.NET Standard** 組件，請建立或新增檔案，並確保**建置動作：EmbeddedResource**。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![設定內嵌資源建置動作](files-images/vs-embeddedresource-sml.png "設定 EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png#lightbox "設定 EmbeddedResource BuildAction")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![內嵌在 PCL 的文字檔，設定內嵌資源建置動作](files-images/xs-embeddedresource-sml.png "設定 EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png#lightbox "設定 EmbeddedResource BuildAction")

-----

`GetManifestResourceStream` 用以存取使用其**資源識別碼**的內嵌檔案。 根據預設，資源識別碼是檔案名稱前置詞加內嵌所在之專案的預設命名空間；在此情況下，組件是 **WorkingWithFiles** 而檔案名稱是 **PCLTextResource.txt**，所以資源識別碼是 `WorkingWithFiles.PCLTextResource.txt`。

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

然後就可以使用 `text` 變數顯示文字，或在程式碼中使用它。 [範例應用程式](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)的這個螢幕擷取畫面顯示使用 `Label` 控制項轉譯的文字。

 [![內嵌在 PCL 的文字檔](files-images/pcltext-sml.png "顯示在應用程式中之內嵌在 PCL 的文字檔")](files-images/pcltext.png#lightbox "顯示在應用程式中之內嵌在 PCL 的文字檔")

載入和還原序列化 XML 一樣簡單。 下列程式碼示範從資源載入並還原序列化 XML 檔案，然後繫結至 `ListView` 以顯示。 XML 檔案包含 `Monkey` 物件的陣列 (已使用範例程式碼定義類別)。

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![內嵌在 PCL 中的 XML 檔，以 ListView 顯示](files-images/pclxml-sml.png "將 XML 檔案內嵌在 PCL 中，以 ListView 顯示")](files-images/pclxml.png#lightbox "將 XML 檔案內嵌在 PCL 中，以 ListView 顯示")

<a name="Embedding_in_Shared_Projects" />

## <a name="embedding-in-shared-projects"></a>在共用專案中內嵌

共用專案也可以將檔案包含為內嵌資源，但因為共用專案的內容會編譯到參考專案，所以用於內嵌檔案資源識別碼的前置詞會變更。 這表示每個平台的每個內嵌檔案資源識別碼可能不相同。

共用專案的這個問題有兩個解決方案：

- **同步專案** - 編輯每個平台的專案屬性，使用**相同的**組件名稱和預設命名空間。 然後，這個值就會「硬式編碼」為共用專案的內嵌資源識別碼前置詞。
- **#if 編譯器指示詞** - 使用編譯器指示詞設定正確的資源識別碼前置詞，並使用該值以動態方式建構正確的資源識別碼。


以下為示範第二個選項的程式碼。 編譯器指示詞用以選取硬式編碼的資源前置詞 (一般和參考專案的預設命名空間相同)。 然後串連資源前置詞與內嵌的資源檔案名稱，使用 `resourcePrefix` 變數建立有效的資源識別碼。

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>組織資源

上述範例假設該檔案內嵌在 .NET Standard 程式庫專案的根目錄中；在這種情況下，資源識別碼就是表單 **Namespace.Filename.Extension** 的資源識別碼，例如 `WorkingWithFiles.PCLTextResource.txt` 和 `WorkingWithFiles.iOS.SharedTextResource.txt`。

您可以使用資料夾組織內嵌的資源。 當內嵌的資源放置在某個資料夾中時，資料夾名稱會成為資源識別碼的一部分 (以句點分隔)，以致資源識別碼格式變成 **Namespace.Folder.Filename.Extension**。 將範例應用程式所用的檔案放到資料夾 **MyFolder** 中，對應的資源識別碼就會變成 `WorkingWithFiles.MyFolder.PCLTextResource.txt` 和 `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`。

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>偵錯內嵌資源

因為有時很難了解為什麼未載入特定的資源，所以下列偵錯程式碼會暫時新增至應用程式，協助確認已正確設定資源。 它會將內嵌在指定組件中的所有已知資源輸出到 [錯誤] 板，協助偵錯資源載入問題。

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

## <a name="summary"></a>總結

本文已示範在裝置上儲存及載入文字，以及載入內嵌資源等一些簡單的檔案作業。 使用 .NET Standard 2.0 就可以在 .NET Standard 程式庫中共用檔案存取碼。

## <a name="related-links"></a>相關連結

- [FilesSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)
- [Xamarin.Forms 範例](https://github.com/xamarin/xamarin-forms-samples)
- [使用 Xamarin.iOS 的檔案系統](~/ios/app-fundamentals/file-system.md)

