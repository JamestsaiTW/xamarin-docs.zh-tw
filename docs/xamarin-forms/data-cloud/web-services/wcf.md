---
title: 使用 Windows Communication Foundation (WCF) Web 服務
description: 這篇文章會示範如何使用 Xamarin.Forms 應用程式從 WCF 簡易物件存取通訊協定 (SOAP) 服務。
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: d170e37b8bf4ce880f9d8f48d30defb42ee6bba2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648015"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>使用 Windows Communication Foundation (WCF) Web 服務

[![下載範例](~/media/shared/download.png) 下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)

_WCF 是 Microsoft 的統一的架構，用於建置服務導向應用程式。它可讓開發人員建置安全、 可靠、 交易，且可互通的分散式應用程式。這篇文章會示範如何使用 Xamarin.Forms 應用程式從 WCF 簡易物件存取通訊協定 (SOAP) 服務。_

WCF 描述具有各種不同合約的服務, 包括:

- **資料合約**– 定義訊息內容的基礎資料結構。
- **訊息合約**– 撰寫從現有的資料合約的訊息。
- **錯誤合約**– 允許指定自訂 SOAP 錯誤。
- **服務合約**– 指定服務支援的作業和訊息所需的互動與每個作業。 它們也會指定可以與每個服務上的作業相關聯的任何自訂的錯誤行為。

ASP.NET Web 服務 (.ASMX) 和 WCF 之間有差異, 但 WCF 支援的功能與 .ASMX 所提供的相同, 也就是透過 HTTP 的 SOAP 訊息。 如需使用 .ASMX 服務的詳細資訊, 請參閱[使用 ASP.NET Web 服務 (.asmx)](~/xamarin-forms/data-cloud/web-services/asmx.md)。

> [!IMPORTANT]
> WCF 的 Xamarin 平臺支援是透過 HTTP/HTTPS 使用`BasicHttpBinding`類別, 限制為文字編碼的 SOAP 訊息。
>
> WCF 支援需要使用僅在 Windows 環境中提供的工具來產生 proxy 並裝載 TodoWCFService。 建立和測試 iOS 應用程式需要在 Windows 電腦或 Azure web 服務上部署 TodoWCFService。
>
> Xamarin Forms 原生應用程式通常會與 .NET Standard 類別庫共用程式碼。 不過, .NET Core 目前不支援 WCF, 因此共用的專案必須是舊版的可移植類別庫。 如需 .NET Core 中 WCF 支援的詳細資訊, 請參閱[為伺服器應用程式選擇 .Net core 和 .NET Framework](/dotnet/standard/choosing-core-framework-server)。

範例應用程式解決方案包含可在本機執行的 WCF 服務, 如下列螢幕擷取畫面所示:

![](wcf-images/portal.png "範例應用程式")

> [!NOTE]
> 在 iOS 9 和更新版本中，App Transport Security (ATS) 會強制執行安全的連線 （例如應用程式的後端伺服器） 的網際網路資源與應用程式，藉此防止意外洩漏機密資訊。 針對 iOS 9 所建置的應用程式中的預設會啟用 ATS，因為所有連線將會都受限於 ATS 安全性需求。 如果連線不符合這些需求，它們將會失敗並發生例外狀況。
>
> 如果無法使用，可以共選擇 ATS`HTTPS`通訊協定和安全的網際網路資源的通訊。 這可以藉由更新應用程式的達成**Info.plist**檔案。 如需詳細資訊，請參閱[應用程式的傳輸安全性](~/ios/app-fundamentals/ats.md)。

## <a name="consume-the-web-service"></a>使用 web 服務

WCF 服務提供下列作業：

|運算|描述|參數|
|--- |--- |--- |
|GetTodoItems|取得待辦事項的清單|
|CreateTodoItem|建立新的待辦事項項目|XML 序列化 TodoItem|
|EditTodoItem|更新待辦事項|XML 序列化 TodoItem|
|DeleteTodoItem|刪除待辦事項|XML 序列化 TodoItem|

如需有關使用應用程式中的資料模型的詳細資訊，請參閱[將資料模型化](~/xamarin-forms/data-cloud/web-services/introduction.md)。

A *proxy*必須產生取用 WCF 服務，可讓應用程式連接至服務。 Proxy 的建構方式取用的服務中繼資料定義的方法和相關聯的服務組態。 此中繼資料會產生 web 服務的 Web 服務描述語言 (WSDL) 文件的形式公開。 在 Visual Studio 2017 中使用 Microsoft WCF Web Service Reference Provider，將 web 服務的服務參考新增至.NET Standard 程式庫，可以建立 proxy。 建立 Visual Studio 2017 中使用 Microsoft WCF Web Service Reference Provider proxy 的替代方式是使用 ServiceModel Metadata Utility Tool (svcutil.exe)。 如需詳細資訊，請參閱 < [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)。

產生的 proxy 類別會提供方法來取用 web 服務使用非同步程式設計模型 (APM) 設計模式。 在此模式中，非同步作業會實作為兩個方法，名為*BeginOperationName*並*EndOperationName*，其開始和結束非同步作業。

*BeginOperationName*方法開始非同步作業，並傳回該物件會實作`IAsyncResult`介面。 之後呼叫*BeginOperationName*，應用程式可以繼續呼叫的執行緒上執行指令，而非同步作業會在執行緒集區執行緒。

每次呼叫*BeginOperationName*，應用程式也應該呼叫*EndOperationName*取得作業的結果。 傳回值*EndOperationName*同步 web 服務方法所傳回的類型相同。 例如，`EndGetTodoItems`方法傳回的集合`TodoItem`執行個體。 *EndOperationName*方法還包括`IAsyncResult`應該設定為對應呼叫所傳回的執行個體的參數*BeginOperationName*方法。

Task Parallel Library (TPL) 可以簡化使用 APM begin/end 方法組，藉由將封裝中相同的非同步作業的程序`Task`物件。 此封裝由多個多載提供`TaskFactory.FromAsync`方法。

如需 APM 的詳細資訊請參閱 <<c0> [ 非同步程式設計模型](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx)並[TPL 和傳統.NET Framework 非同步程式設計](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx)MSDN 上。

### <a name="create-the-todoserviceclient-object"></a>建立 TodoServiceClient 物件

產生的 proxy 類別會提供`TodoServiceClient`類別，用來透過 HTTP 與 WCF 服務通訊。 它會提供功能來叫用 web 服務方法，如非同步作業，從 URI 所識別的服務執行個體。 如需有關非同步作業的詳細資訊，請參閱 <<c0> [ 非同步支援概觀](~/cross-platform/platform/async.md)。

`TodoServiceClient` ，讓物件存留的只要應用程式需要取用 WCF 服務，如下列程式碼範例所示，執行個體在類別層級宣告：

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

`TodoServiceClient`執行個體已在使用繫結資訊和端點位址。 繫結用來指定傳輸、 編碼及所需的應用程式和服務彼此通訊的通訊協定詳細資料。 `BasicHttpBinding`指定文字編碼的 SOAP 訊息會透過 HTTP 傳輸通訊協定傳送。 指定端點位址可讓應用程式連接到 WCF 服務的不同執行個體，前提是有多個已發行的執行個體。

如需有關如何設定服務參考的詳細資訊，請參閱 <<c0> [ 設定服務參考](~/cross-platform/data-cloud/web-services/index.md#wcf)。

### <a name="create-data-transfer-objects"></a>建立資料傳輸物件

範例應用程式使用`TodoItem`模型資料的類別。 若要儲存`TodoItem`它必須先轉換成產生的 proxy web 服務中的項目`TodoItem`型別。 這可以藉由`ToWCFServiceTodoItem`方法，如下列程式碼範例所示：

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

這個方法只會建立新`TodoWCFService.TodoItem`執行個體，並將每個屬性設定為相同的屬性，從`TodoItem`執行個體。

同樣地，從 web 服務擷取資料時，必須將它從轉換產生的 proxy`TodoItem`輸入至`TodoItem`執行個體。 這可完成`FromWCFServiceTodoItem`方法，如下列程式碼範例所示：

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

這個方法只擷取資料，從產生的 proxy`TodoItem`輸入，並將它設定在新建立`TodoItem`執行個體。

### <a name="retrieve-data"></a>取出資料

`TodoServiceClient.BeginGetTodoItems`並`TodoServiceClient.EndGetTodoItems`方法來呼叫`GetTodoItems`web 服務所提供的作業。 這些非同步方法，都會封裝在`Task`物件，如下列程式碼範例所示：

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems)
  {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync`方法會建立`Task`執行`TodoServiceClient.EndGetTodoItems`方法一次`TodoServiceClient.BeginGetTodoItems`方法完成時，使用`null`參數，指出正在將資料傳遞至`BeginGetTodoItems`委派。 最後，windows 7`TaskCreationOptions`列舉會指定應建立和執行工作的預設行為。

`TodoServiceClient.EndGetTodoItems`方法會傳回`ObservableCollection`的`TodoWCFService.TodoItem`執行個體，然後轉換成`List`的`TodoItem`顯示的執行個體。

### <a name="create-data"></a>建立資料

`TodoServiceClient.BeginCreateTodoItem`並`TodoServiceClient.EndCreateTodoItem`方法來呼叫`CreateTodoItem`web 服務所提供的作業。 這些非同步方法，都會封裝在`Task`物件，如下列程式碼範例所示：

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`方法會建立`Task`執行`TodoServiceClient.EndCreateTodoItem`方法一次`TodoServiceClient.BeginCreateTodoItem`方法完成時，使用`todoItem`參數所傳入資料`BeginCreateTodoItem`委派指定`TodoItem`建立 web 服務。 最後，windows 7`TaskCreationOptions`列舉會指定應建立和執行工作的預設行為。

Web 服務擲回`FaultException`如果無法建立`TodoItem`，這由應用程式。

### <a name="update-data"></a>更新資料

`TodoServiceClient.BeginEditTodoItem`並`TodoServiceClient.EndEditTodoItem`方法來呼叫`EditTodoItem`web 服務所提供的作業。 這些非同步方法，都會封裝在`Task`物件，如下列程式碼範例所示：

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`方法會建立`Task`執行`TodoServiceClient.EndEditTodoItem`方法一次`TodoServiceClient.BeginCreateTodoItem`方法完成時，使用`todoItem`參數所傳入資料`BeginEditTodoItem`委派指定`TodoItem`更新的 web 服務。 最後，windows 7`TaskCreationOptions`列舉會指定應建立和執行工作的預設行為。

Web 服務擲回`FaultException`找出或更新時`TodoItem`，這由應用程式。

### <a name="delete-data"></a>刪除資料

`TodoServiceClient.BeginDeleteTodoItem`並`TodoServiceClient.EndDeleteTodoItem`方法來呼叫`DeleteTodoItem`web 服務所提供的作業。 這些非同步方法，都會封裝在`Task`物件，如下列程式碼範例所示：

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync`方法會建立`Task`執行`TodoServiceClient.EndDeleteTodoItem`方法一次`TodoServiceClient.BeginDeleteTodoItem`方法完成時，使用`id`參數所傳入資料`BeginDeleteTodoItem`委派指定`TodoItem`要刪除 web 服務。 最後，windows 7`TaskCreationOptions`列舉會指定應建立和執行工作的預設行為。

Web 服務擲回`FaultException`找出或刪除時`TodoItem`，這由應用程式。

## <a name="configure-remote-access-to-iis-express"></a>設定遠端存取 IIS Express
在 Visual Studio 2017 或 Visual Studio 2019 中, 您應該能夠在不進行其他設定的電腦上測試 UWP 應用程式。 測試 Android 和 iOS 用戶端可能需要本節中的其他步驟。 如需詳細資訊, 請參閱[從 IOS 模擬器和 Android 模擬器連接到本機 Web 服務](~/cross-platform/deploy-test/connect-to-local-web-services.md)。

根據預設, IIS Express 只會回應對`localhost`的要求。 遠端裝置 (例如 Android 裝置、iPhone 或甚至是模擬器) 將無法存取您的本機 WCF 服務。 您將需要知道您在區域網路上的 Windows 10 工作站 IP 位址。 基於此範例的目的, 假設您的工作站具有 IP 位址`192.168.1.143`。 下列步驟說明如何設定 Windows 10 和 IIS Express 以接受遠端連線, 並從實體或虛擬裝置連線至服務:

1. **將例外狀況新增至 Windows 防火牆**。 您必須透過 Windows 防火牆開啟埠, 子網中的應用程式才能用來與 WCF 服務進行通訊。 建立輸入規則, 以在防火牆中開啟埠49393。 從系統管理命令提示字元中, 執行下列命令:
    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **設定 IIS Express 以接受遠端連線**。 您可以在 **[方案目錄\.] vs\config\applicationhost.config**中編輯 IIS Express 的設定檔, 以設定 IIS Express。尋找名稱`TodoWCFService`為的元素。`site` 看起來應該類似下列 XML:

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
        </bindings>
    </site>
    ```

    您將需要新增兩個`binding`元素, 以開啟埠49393至外部流量和 Android 模擬器。 系結會使用`[IP address]:[port]:[hostname]`指定 IIS Express 如何回應要求的格式。 外部要求的主機名稱必須指定為`binding`。 將下列 XML 新增至`bindings`元素, 並將 ip 位址取代為您自己的 ip 位址:

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    變更之後, `bindings`元素看起來應該如下所示:

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
            <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
            <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
        </bindings>
    </site>
    ```

    >[!IMPORTANT]
    >根據預設, 基於安全性理由, IIS Express 不會接受來自外部來源的連接。 若要啟用遠端裝置的連接, 您必須以系統管理許可權執行 IIS Express。 若要這麼做, 最簡單的方法是使用系統管理許可權來執行 Visual Studio 2017。 這會在執行 TodoWCFService 時, 以系統管理許可權啟動 IIS Express。

    完成這些步驟之後, 您應該能夠執行 TodoWCFService, 並從子網的其他裝置進行連線。 您可以藉由執行您的應用程式並`http://localhost:49393/TodoService.svc`造訪來進行測試。 如果您在流覽該 URL 時收到不正確的**要求**錯誤`bindings` , 您在 IIS Express 設定中可能不正確 (要求 IIS Express 但遭到拒絕)。 如果您收到不同的錯誤, 可能是您的應用程式未執行, 或您的防火牆設定不正確。

    若要讓 IIS Express 繼續執行並提供服務, 請關閉 **專案屬性 > Web > 調試**程式 中的 **編輯後繼續** 選項。

1. **自訂用來存取服務的端點裝置**。 此步驟牽涉到設定在實體或模擬裝置上執行的用戶端應用程式, 以存取 WCF 服務。

    Android 模擬器會利用內部 proxy, 讓模擬器無法直接存取主機電腦的`localhost`位址。 相反地, 模擬器`10.0.2.2`上的位址會透過內部`localhost` proxy 路由傳送至主機電腦上的。 這些代理的要求在`127.0.0.1`要求標頭中會有作為主機名稱, 這就是您在上述步驟中為此主機名稱建立 IIS Express 系結的原因。

    IOS 模擬器會在 Mac 組建主機上執行, 即使您使用[適用于 Windows 的遠端 iOS](~/tools/ios-simulator/index.md)模擬器也一樣。 來自模擬器的網路要求會在區域網路上將您的工作站 IP 做為主機名稱 (在此範例`192.168.1.143`中為), 但您的實際 IP 位址可能會不同。 這就是您在上述步驟中為此主機名稱建立 IIS Express 系結的原因。

    請確定`SoapUrl` TodoWCF (便攜) 專案中**Constants.cs**檔案中的屬性具有您的網路的正確值:

    ```csharp
    public static string SoapUrl
    {
        get
        {
            var defaultUrl = "http://localhost:49393/TodoService.svc";

            if (Device.RuntimePlatform == Device.Android)
            {
                defaultUrl = "http://10.0.2.2:49393/TodoService.svc";
            }
            else if (Device.RuntimePlatform == Device.iOS)
            {
                defaultUrl = "http://192.168.1.143:49393/TodoService.svc";
            }

            return defaultUrl;
        }
    }
    ```

    使用適當的端點設定**Constants.cs**之後, 您應該能夠從實體或虛擬裝置連接到在 Windows 10 工作站上執行的 TodoWCFService。

## <a name="related-links"></a>相關連結

- [TodoWCF （範例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [如何：建立 Windows Communication Foundation 用戶端](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [System.servicemodel 中繼資料公用程式工具 (svcutil .exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
