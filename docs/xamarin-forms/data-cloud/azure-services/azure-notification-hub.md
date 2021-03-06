---
title: 使用 Azure 通知中樞和 Xamarin 來傳送和接收推播通知
description: 本文說明如何使用 Azure 通知中樞, 將跨平臺推播通知傳送至 Xamarin. Forms 應用程式。
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 05/23/2019
ms.openlocfilehash: c4237e9315ccc095abc72fdec24d58ffe1faebdf
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739234"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>使用 Azure 通知中樞和 Xamarin 來傳送和接收推播通知

[![下載範例](~/media/shared/download.png)下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurenotificationhub/)

推播通知會將來自後端系統的資訊傳遞至行動應用程式。 Apple、Google 和其他平臺各有自己的推播通知服務 (PNS)。 Azure 通知中樞可讓您集中跨平臺的通知, 讓您的後端應用程式可以與單一中樞進行通訊, 這會負責將通知散發到每個平臺特定的 PNS。

遵循下列步驟, 將 Azure 通知中樞整合到行動應用程式:

1. [設定推播 Notification Services 和 Azure 通知中樞](#set-up-push-notification-services-and-azure-notification-hub)。
1. [瞭解如何使用範本和標記](#register-templates-and-tags-with-the-azure-notification-hub)。
1. [建立跨平臺的 Xamarin Forms 應用程式](#xamarinforms-application-functionality)。
1. [針對推播通知設定原生 Android 專案](#configure-the-android-application-for-notifications)。
1. [設定推播通知的原生 iOS 專案](#configure-ios-for-notifications)。
1. [使用 Azure 通知中樞的測試通知](#test-notifications-in-the-azure-portal)。
1. [建立後端應用程式以傳送通知](#create-a-notification-dispatcher)。

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>設定推播 Notification Services 和 Azure 通知中樞

整合 Azure 通知中樞與 Xamarin。表單行動應用程式類似于整合 Azure 通知中樞與 Xamarin 原生應用程式。 遵循[使用 Azure 通知中樞將通知推送至 Xamarin](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging)中的 Firebase 主控台步驟來設定**FCM 應用程式**。 使用 Xamarin. Android 教學課程來完成下列步驟:

1. 定義範例中使用的 Android 套件`com.xamarin.notifysample`名稱, 例如。
1. 從 Firebase 主控台下載**google-服務。** 您會在未來的步驟中將此檔案新增至 Android 應用程式。
1. 建立 Azure 通知中樞實例, 並為其命名。 本文和範例會使用`xdocsnotificationhub`做為中樞名稱。
1. 複製 FCM**伺服器金鑰**, 並將它儲存為 Azure 通知中樞中**Google (GCM/FCM)** 下的**API 金鑰**。

下列螢幕擷取畫面顯示 Azure 通知中樞的 Google 平臺設定:

![Azure 通知中樞 Google 設定的螢幕擷取畫面](azure-notification-hub-images/fcm-notification-hub-config.png "Azure 通知中樞 Google")設定

您將需要 macOS 機器, 才能完成 iOS 裝置的設定。 遵循[使用 Azure 通知中樞將通知推送至 Xamarin](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file)的初始步驟來設定 APNS。 使用 Xamarin 教學課程來完成下列步驟:

1. 定義 iOS 套件組合識別碼。 本文和範例會使用`com.xamarin.notifysample`做為套件組合識別碼。
1. 建立憑證簽署要求 (CSR) 檔案, 並使用它來產生推播通知憑證。
1. 將推播通知憑證上傳至 Azure 通知中樞的**Apple (APNS)** 底下。

下列螢幕擷取畫面顯示 Azure 通知中樞的 Apple 平臺設定:

![Azure 通知中樞 Apple 設定的螢幕擷取畫面](azure-notification-hub-images/apns-notification-hub-config.png "Azure 通知中樞 Apple")設定

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>使用 Azure 通知中樞註冊範本和標記

Azure 通知中樞需要行動應用程式向中樞註冊、定義範本和訂閱標記。 註冊會將平臺特定的 PNS 控制碼連結至 Azure 通知中樞內的識別碼。 若要深入瞭解註冊, 請參閱[註冊管理](/azure/notification-hubs/notification-hubs-push-notification-registration-management)。

範本可讓裝置指定參數化訊息範本。 內送訊息可以自訂每個裝置, 每個標記。 若要深入瞭解範本, 請參閱[範本](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)。

標記可以用來訂閱訊息類別, 例如新聞、體育和氣象。 為了簡單起見, 範例應用程式會使用名`messageParam`為的單一參數和名`default`為的單一標記來定義預設範本。 在更複雜的系統中, 您可以使用使用者特定的標籤, 將裝置的使用者訊息用於個人化通知。 若要深入瞭解標記, 請參閱[路由與標記運算式](/azure/notification-hubs/notification-hubs-tags-segment-push-message)。

若要成功接收訊息, 每個原生應用程式都必須執行下列步驟:

1. 從平臺 PNS 取得 PNS 控制碼或 token。
1. 向 Azure 通知中樞註冊 PNS 控制碼。
1. 指定範本, 其中包含與外寄訊息相同的參數。
1. 訂閱外寄訊息的目標標記。

針對[設定通知的 Android 應用程式](#configure-the-android-application-for-notifications)和[設定通知的 iOS](#configure-ios-for-notifications)一節中的每個平臺, 會進一步詳細說明這些步驟。

## <a name="xamarinforms-application-functionality"></a>Xamarin. Forms 應用程式功能

範例 Xamarin. Forms 應用程式會顯示推播通知訊息的清單。 這是透過`AddMessage`方法來達成, 這會將指定的推播通知訊息新增至 UI。 這個方法也可防止將重複的訊息加入至 UI, 並在主執行緒上執行, 以便從任何執行緒呼叫它。 下列程式碼顯示 `AddMessage` 方法：

```csharp
public void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (messageDisplay.Children.OfType<Label>().Where(c => c.Text == message).Any())
        {
            // Do nothing, an identical message already exists
        }
        else
        {
            Label label = new Label()
            {
                Text = message,
                HorizontalOptions = LayoutOptions.CenterAndExpand,
                VerticalOptions = LayoutOptions.Start
            };
            messageDisplay.Children.Add(label);
        }
    });
}
```

範例應用程式包含**AppConstants.cs**檔案, 此檔案會定義平臺專案所使用的屬性。 此檔案必須使用來自 Azure 通知中樞的值進行自訂。 下列程式碼顯示**AppConstants.cs**檔案:

```csharp
public static class AppConstants
{
    public static string NotificationChannelName { get; set; } = "XamarinNotifyChannel";
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string ListenConnectionString { get; set; } = "< Insert your DefaultListenSharedAccessSignature >";
    public static string DebugTag { get; set; } = "XamarinNotify";
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string FCMTemplateBody { get; set; } = "{\"data\":{\"message\":\"$(messageParam)\"}}";
    public static string APNTemplateBody { get; set; } = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";
}
```

在中`AppConstants`自訂下列值, 以將範例應用程式連線至您的 Azure 通知中樞:

* `NotificationHubName`：使用您在 Azure 入口網站中建立的 Azure 通知中樞名稱。
* `ListenConnectionString`：此值可在 Azure 通知中樞的 [**存取原則**] 下找到。

下列螢幕擷取畫面顯示這些值位於 Azure 入口網站中的位置:

![Azure 通知中樞存取原則的螢幕擷取畫面](azure-notification-hub-images/notification-hub-access-policy.png "Azure 通知中樞存取原則")

## <a name="configure-the-android-application-for-notifications"></a>設定 Android 應用程式的通知

請完成下列步驟, 將 Android 應用程式設定為接收和處理通知:

1. 設定 Android**套件名稱**, 使其符合 Firebase 主控台中的套件名稱。
1. 安裝下列 NuGet 套件, 以與 Google Play、Firebase 和 Azure 通知中樞互動:
    1. GooglePlayServices。
    1. Firebase. 訊息。
    1. Xamarin.Azure.NotificationHubs.Android.
1. 將您在 FCM 安裝期間下載的檔案複製到專案中, 並將組建動作`GoogleServicesJson`設為。 `google-services.json`
1. [設定 androidmanifest.xml 與 Firebase 通訊](#configure-android-manifest)。
1. [使用 Firebase 和 Azure 通知中樞來`FirebaseInstanceIdService`註冊應用程式](#register-using-a-custom-firebaseinstanceidservice)。
1. [使用`FirebaseMessagingService`來處理訊息](#process-messages-with-a-firebasemessagingservice)。
1. [將傳入通知加入至 Xamarin. FORMS UI](#add-incoming-notifications-to-the-xamarinforms-ui)。

> [!NOTE]
> **GoogleServicesJson**組建動作是**GooglePlayServices 基底**NuGet 套件的一部分。 Visual Studio 2019 會在啟動期間設定可用的組建動作。 如果您看不到 [ **GoogleServicesJson** ] 做為組建動作, 請在安裝 NuGet 套件後重新開機 Visual Studio 2019。

### <a name="configure-android-manifest"></a>設定 Android 資訊清單

元素內的`receiver`元素可讓應用程式與 Firebase 通訊。 `application` `uses-permission`元素可讓應用程式處理訊息, 並向 Azure 通知中樞註冊。 完整的**androidmanifest.xml**看起來應該類似下列範例:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="YOUR_PACKAGE_NAME" android:installLocation="auto">
  <uses-sdk android:minSdkVersion="21" />
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
  <application android:label="Notification Hub Sample">
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
      <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
      </intent-filter>
    </receiver>
  </application>
</manifest>
```

### <a name="register-using-a-custom-firebaseinstanceidservice"></a>使用自訂 FirebaseInstanceIdService 進行註冊

Firebase 會發出可唯一識別 PNS 上裝置的權杖。 權杖的存留期很長, 但偶爾會重新整理。 發行或重新整理權杖時, 應用程式必須向 Azure 通知中樞註冊其新權杖。 註冊是由衍生自之`FirebaseInstanceIdService`類別的實例所處理。

在範例應用程式中`FirebaseRegistrationService` , 類別繼承`FirebaseInstanceIdService`自。 這個類別具有`IntentFilter`包含`com.google.firebase.INSTANCE_ID_EVENT`的, 可讓 Android OS 在 Firebase 發行權杖`OnTokenRefresh`時自動呼叫。

下列程式碼顯示範例應用`FirebaseInstanceIdService`程式中的自訂:

```csharp
[Service]
[IntentFilter(new [] { "com.google.firebase.INSTANCE_ID_EVENT"})]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    public override void OnTokenRefresh()
    {
        string token = FirebaseInstanceId.Instance.Token;

        // NOTE: logging the token is not recommended in production but during
        // development it is useful to test messages directly from Firebase
        Log.Info(AppConstants.DebugTag, $"Token received: {token}");

        SendRegistrationToServer(token);
    }

    void SendRegistrationToServer(string token)
    {
        try
        {
            NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

            // register device with Azure Notification Hub using the token from FCM
            Registration reg = hub.Register(token, AppConstants.SubscriptionTags);

            // subscribe to the SubscriptionTags list with a simple template.
            string pnsHandle = reg.PNSHandle;
            var cats = string.Join(", ", reg.Tags);
            var temp = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
        }
        catch (Exception e)
        {
            Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
        }
    }
}
```

中的`SendRegistrationToServer`方法會向 Azure 通知中樞註冊裝置,並使用範本來訂閱標記。`FirebaseRegistrationClass` 範例應用程式會定義名`default`為的單一標記, 以及`messageParam`在**AppConstants.cs**檔案中具有單一參數的範本。 如需有關註冊、標記和範本的詳細資訊, 請參閱[使用 Azure 通知中樞註冊範本和標記](#register-templates-and-tags-with-the-azure-notification-hub)

### <a name="process-messages-with-a-firebasemessagingservice"></a>以 FirebaseMessagingService 處理訊息

內送訊息會路由傳送`FirebaseMessagingService`至實例, 並在其中轉換成本機通知。 範例應用程式中的 Android 專案包含名`FirebaseService`為的類別, 其繼承自。 `FirebaseMessagingService` 這個類別具有`IntentFilter`包含`com.google.firebase.MESSAGING_EVENT`的, 可讓 Android OS 在收到推播`OnMessageReceived`通知訊息時自動呼叫。

下列範例會顯示範例`FirebaseService`應用程式中的:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
    public override void OnMessageReceived(RemoteMessage message)
    {
        base.OnMessageReceived(message);
        string messageBody = string.Empty;

        if (message.GetNotification() != null)
        {
            messageBody = message.GetNotification().Body;
        }

        // NOTE: test messages sent via the Azure portal will be received here
        else
        {
            messageBody = message.Data.Values.First();
        }

        // convert the incoming message to a local notification
        SendLocalNotification(messageBody);

        // send the incoming message directly to the MainPage
        SendMessageToMainPage(messageBody);
    }

    void SendLocalNotification(string body)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        intent.PutExtra("message", body);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetContentTitle("XamarinNotify Message")
            .SetSmallIcon(Resource.Drawable.ic_launcher)
            .SetContentText(body)
            .SetAutoCancel(true)
            .SetShowWhen(false)
            .SetContentIntent(pendingIntent);

        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            notificationBuilder.SetChannelId(AppConstants.NotificationChannelName);
        }

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }

    void SendMessageToMainPage(string body)
    {
        (App.Current.MainPage as MainPage)?.AddMessage(body);
    }
}
```

傳入訊息會使用`SendLocalNotification`方法轉換成本機通知。 這個方法會建立新`Intent`的, 並將訊息內容放`Intent`入中`string`做為`Extra`。 當使用者按下本機通知時, 不論應用程式是在前景還是背景中, `MainActivity`都會啟動, 並可`Intent`透過物件存取訊息內容。

本機通知和`Intent`範例要求使用者採取動作以在通知上點擊。 當使用者應該在應用程式狀態變更之前採取動作時, 這是很好的做法。 不過, 在某些情況下, 您可能會想要存取訊息資料, 而不需要使用者動作。 先前的範例也會`MainPage` `SendMessageToMainPage`使用方法, 直接將訊息傳送至目前的實例。 在生產環境中, 如果您針對單一訊息類型執行這兩種`MainPage`方法, 當使用者按下通知時, 該物件將會收到重複的訊息。

> [!NOTE]
> Android 應用程式只會在背景或前景中執行時, 才會收到推播通知。 若要在主要`Activity`未執行時接收推播通知, 您必須實作用於此範例範圍外的服務。 如需詳細資訊, 請參閱[建立 Android 服務](/xamarin/android/app-fundamentals/services/)

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>將傳入通知新增至 Xamarin. Forms UI

類別`MainActivity`需要取得處理通知和管理傳入訊息資料的許可權。 下列程式碼顯示完整`MainActivity`的執行方式:

```csharp
[Activity(Label = "NotificationHubSample", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, LaunchMode = LaunchMode.SingleTop)]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);

        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App());

        if (IsPlayServiceAvailable() == false)
        {
            throw new Exception("This device does not have Google Play Services and cannot receive push notifications.");
        }

        CreateNotificationChannel();
    }

    protected override void OnNewIntent(Intent intent)
    {
        if (intent.Extras != null)
        {
            var message = intent.GetStringExtra("message");
            (App.Current.MainPage as MainPage)?.AddMessage(message);
        }

        base.OnNewIntent(intent);
    }

    bool IsPlayServiceAvailable()
    {
        int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.Success)
        {
            if (GoogleApiAvailability.Instance.IsUserResolvableError(resultCode))
                Log.Debug(AppConstants.DebugTag, GoogleApiAvailability.Instance.GetErrorString(resultCode));
            else
            {
                Log.Debug(AppConstants.DebugTag, "This device is not supported");
            }
            return false;
        }
        return true;
    }

    void CreateNotificationChannel()
    {
        // Notification channels are new as of "Oreo".
        // There is no need to create a notification channel on older versions of Android.
        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            var channelName = AppConstants.NotificationChannelName;
            var channelDescription = String.Empty;
            var channel = new NotificationChannel(channelName, channelName, NotificationImportance.Default)
            {
                Description = channelDescription
            };

            var notificationManager = (NotificationManager)GetSystemService(NotificationService);
            notificationManager.CreateNotificationChannel(channel);
        }
    }
}
```

屬性會將應用程式`LaunchMode`設定`SingleTop`為。 `Activity` 此啟動模式會告知 Android OS 只允許此活動的單一實例。 使用此啟動模式時, `Intent`傳入的`OnNewIntent`資料會路由傳送至方法, 這會解壓縮訊息資料, `MainPage`並透過`AddMessage`方法將它傳送至實例。 如果您的應用程式使用不同的啟動模式, 它`Intent`必須以不同的方式處理資料。

方法會使用名`IsPlayServiceAvailable`為的 helper 方法, 以確保裝置支援 Google Play 服務。 `OnCreate` 不支援 Google Play 服務的模擬器或裝置無法從 Firebase 接收推播通知。

## <a name="configure-ios-for-notifications"></a>設定 iOS 的通知

設定 iOS 應用程式以接收通知的流程如下:

1. 在**plist**檔案中設定套件組合**識別碼**, 以符合布建設定檔中所使用的值。
1. 將 [**啟用推播通知**] 選項新增至**plist**檔案。
1. 將**NotificationHubs**新增至您的專案。
1. [向 APNS 註冊通知](#register-for-notifications-with-apns)。
1. 向[Azure 通知中樞註冊應用程式並訂閱標記](#register-with-azure-notification-hub-and-subscribe-to-tags)。
1. [將 APNS 通知新增至 Xamarin. FORMS UI](#add-apns-notifications-to-xamarinforms-ui)。

下列螢幕擷取畫面顯示 Visual Studio 中的**plist**檔案中選取 [**啟用推播通知**] 選項:

![推播通知權利的螢幕擷取畫面](azure-notification-hub-images/push-notification-entitlement.png "推播通知權利")

### <a name="register-for-notifications-with-apns"></a>向 APNS 註冊通知

必須`FinishedLaunching`覆寫**AppDelegate.cs**檔案中的方法, 才能註冊遠端通知。 註冊會根據裝置上使用的 iOS 版本而有所不同。 範例應用程式中的 iOS 專案會覆`FinishedLaunching`寫要呼叫`RegisterForRemoteNotifications`的方法, 如下列範例所示:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();
    LoadApplication(new App());

    base.FinishedLaunching(app, options);

    RegisterForRemoteNotifications();

    return true;
}

void RegisterForRemoteNotifications()
{
    // register for remote notifications based on system version
    if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
    {
        UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert |
            UNAuthorizationOptions.Sound |
            UNAuthorizationOptions.Sound,
            (granted, error) =>
            {
                if (granted)
                    InvokeOnMainThread(UIApplication.SharedApplication.RegisterForRemoteNotifications);
            });
    }
    else if (UIDevice.CurrentDevice.CheckSystemVersion(8, 0))
    {
        var pushSettings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
        new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(pushSettings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();
    }
    else
    {
        UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
        UIApplication.SharedApplication.RegisterForRemoteNotificationTypes(notificationTypes);
    }
}
```

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>向 Azure 通知中樞註冊並訂閱標記

當裝置在`FinishedLaunching`方法期間成功註冊遠端通知時, iOS 會`RegisteredForRemoteNotifications`呼叫方法。 應該覆寫此方法, 以執行下列動作:

1. 具現化。 `SBNotificationHub`
1. 取消註冊任何現有的註冊。
1. 向您的通知中樞註冊裝置。
1. 使用範本訂閱特定標記。

如需註冊裝置、範本和標記的詳細資訊, 請參閱[使用 Azure 通知中樞註冊範本和標記](#register-templates-and-tags-with-the-azure-notification-hub)。 下列程式碼示範裝置和範本的註冊:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAllAsync(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplateAsync(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                if (errorCallback != null)
                {
                    Debug.WriteLine($"RegisterTemplateAsync error: {errorCallback}");
                }
            }
        });
    });
}
```

> [!NOTE]
> 註冊遠端通知可能會在沒有網路連線的情況下失敗。 您可以選擇覆寫`FailedToRegisterForRemoveNotifications`方法來處理註冊失敗。

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>將 APNS 通知新增至 Xamarin。表單 UI

當裝置收到遠端通知時, iOS 會呼叫`ReceivedRemoteNotification`方法。 傳入訊息 JSON 會轉換成`NSDictionary`物件, `ProcessNotification`而方法會從字典中解壓縮值, 並將其傳送`MainPage`至 Xamarin 實例。 會覆寫`ProcessNotification`方法以呼叫, 如下列程式碼所示: `ReceivedRemoteNotifications`

```csharp
public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
{
    ProcessNotification(userInfo, false);
}

void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
{
    // make sure we have a payload
    if (options != null && options.ContainsKey(new NSString("aps")))
    {
        // get the APS dictionary and extract message payload. Message JSON will be converted
        // into a NSDictionary so more complex payloads may require more processing
        NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
        string payload = string.Empty;
        NSString payloadKey = new NSString("alert");
        if (aps.ContainsKey(payloadKey))
        {
            payload = aps[payloadKey].ToString();
        }

        if (!string.IsNullOrWhiteSpace(payload))
        {
            (App.Current.MainPage as MainPage)?.AddMessage(payload);
        }

    }
    else
    {
        Debug.WriteLine($"Received request to process notification but there was no payload.");
    }
}
```

## <a name="test-notifications-in-the-azure-portal"></a>Azure 入口網站中的測試通知

Azure 通知中樞可讓您檢查應用程式是否可以接收測試訊息。 [通知中樞] 中的 [**測試傳送**] 區段可讓您選擇目標平臺, 並傳送訊息。 將**傳送至**標籤運算式設定`default`為, 會將訊息傳送至已為`default`標記註冊範本的應用程式。 按一下 [**傳送**] 按鈕會產生一份報告, 其中包含與訊息一起到達的裝置數目。 下列螢幕擷取畫面顯示 Azure 入口網站中的 Android 通知測試:

![Azure 通知中樞測試訊息的螢幕擷取畫面](azure-notification-hub-images/azure-notification-hub-test-send.png "Azure 通知中樞測試訊息")

### <a name="testing-tips"></a>測試秘訣

1. 測試應用程式能否接收推播通知時, 您必須使用實體裝置。 Android 和 iOS 虛擬裝置可能未正確設定, 而無法接收推播通知。
1. Android 應用程式範例會在發出 Firebase token 時, 註冊其權杖和範本一次。 在測試期間, 您可能需要要求新的權杖, 並使用 Azure 通知中樞重新註冊。 強制執行此工作的最佳方式是清理您的`bin`專案、刪除和`obj`資料夾, 並從裝置卸載應用程式, 然後再重建和部署。
1. 推播通知流程的許多部分會以非同步方式執行。 這可能會導致中斷點未被叫用, 或未依預期的順序叫用。 使用裝置或 debug 記錄來追蹤執行, 而不中斷應用程式流程。 使用中`DebugTag` `Constants`指定的來篩選 Android 裝置記錄。

## <a name="create-a-notification-dispatcher"></a>建立通知發送器

Azure 通知中樞可讓您的後端應用程式將通知分派給跨平臺的裝置。 此範例示範使用**NotificationDispatcher**主控台應用程式的通知分派。 應用程式包含**DispatcherConstants.cs**檔案, 此檔案會定義下列屬性:

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

您必須設定**DispatcherConstants.cs**以符合您的 Azure 通知中樞設定。 `SubscriptionTags`屬性的值應該符合用戶端應用程式中所使用的值。 `NotificationHubName`屬性是 Azure 通知中樞實例的名稱。 屬性是在您的通知中樞**存取原則**中找到的存取金鑰。 `FullAccessConnectionString` 下列螢幕擷取畫面顯示`NotificationHubName`和`FullAccessConnectionString`屬性在 Azure 入口網站中的位置:

![Azure 通知中樞名稱和 FullAccessConnectionString 的螢幕擷取畫面](azure-notification-hub-images/notification-hub-full-access-policy.png "Azure 通知中樞名稱和 FullAccessConnectionString")

主控台應用程式會迴圈處理`SubscriptionTags`每個值, 並使用`NotificationHubClient`類別的實例, 將通知傳送給訂閱者。 下列程式碼顯示主控台應用程式`Program`類別:

``` csharp
class Program
{
    static int messageCount = 0;

    static void Main(string[] args)
    {
        Console.WriteLine($"Press the spacebar to send a message to each tag in {string.Join(", ", DispatcherConstants.SubscriptionTags)}");
        WriteSeparator();
        while (Console.ReadKey().Key == ConsoleKey.Spacebar)
        {
            SendTemplateNotificationsAsync().GetAwaiter().GetResult();
        }
    }

    private static async Task SendTemplateNotificationsAsync()
    {
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(DispatcherConstants.FullAccessConnectionString, DispatcherConstants.NotificationHubName);
        Dictionary<string, string> templateParameters = new Dictionary<string, string>();

        messageCount++;

        // Send a template notification to each tag. This will go to any devices that
        // have subscribed to this tag with a template that includes "messageParam"
        // as a parameter
        foreach (var tag in DispatcherConstants.SubscriptionTags)
        {
            templateParameters["messageParam"] = $"Notification #{messageCount} to {tag} category subscribers!";
            try
            {
                await hub.SendTemplateNotificationAsync(templateParameters, tag);
                Console.WriteLine($"Sent message to {tag} subscribers.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Failed to send template notification: {ex.Message}");
            }
        }

        Console.WriteLine($"Sent messages to {DispatcherConstants.SubscriptionTags.Length} tags.");
        WriteSeparator();
    }

    private static void WriteSeparator()
    {
        Console.WriteLine("==========================================================================");
    }
}
```

執行範例主控台應用程式時, 可以按下空格鍵來傳送訊息。 執行用戶端應用程式的裝置應該會收到編號的通知, 前提是它們已正確設定。

## <a name="related-links"></a>相關連結

* [推播通知範本](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)。
* [裝置註冊管理](/azure/notification-hubs/notification-hubs-push-notification-registration-management)。
* [路由與標記運算式](/azure/notification-hubs/notification-hubs-tags-segment-push-message)。
* [Xamarin. Android Azure 通知中樞教學](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)課程。
* [Xamarin IOS Azure 通知中樞教學](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)課程。
