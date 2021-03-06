---
title: Xamarin Android 行事曆
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 610f9be346b761c4d20e4bbf97ecfcda74c0bee0
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523059"
---
# <a name="xamarinandroid-calendar"></a>Xamarin Android 行事曆


## <a name="calendar-api"></a>行事曆 API

Android 4 引進一組新的行事曆 Api, 支援設計用來讀取或寫入資料至行事曆提供者的應用程式。 這些 Api 支援許多與行事歷數據互動的選項, 包括讀取和寫入事件、出席者和提醒的能力。 藉由在您的應用程式中使用行事曆提供者, 您透過 API 新增的資料會出現在 Android 4 隨附的內建行事曆應用程式中。


## <a name="adding-permissions"></a>新增許可權

在您的應用程式中使用新的行事曆 Api 時, 您必須做的第一件事就是將適當的許可權新增至 Android 資訊清單。 您需要加入的許可權是`android.permisson.READ_CALENDAR`和`android.permission.WRITE_CALENDAR`, 視您正在讀取及/或寫入行事歷數據而定。


## <a name="using-the-calendar-contract"></a>使用行事曆合約

設定許可權之後, 您可以使用`CalendarContract`類別與行事歷數據互動。 這個類別會提供應用程式與行事曆提供者互動時可使用的資料模型。 `CalendarContract`可讓應用程式將 uri 解析為行事曆實體, 例如行事曆和事件。 它也提供一種方式來與每個實體中的各種欄位互動, 例如行事曆的名稱和識別碼, 或是事件的開始和結束日期。

讓我們來看一個使用行事曆 API 的範例。 在此範例中, 我們將探討如何列舉行事曆和其事件, 以及如何將新的活動加入行事曆。


## <a name="listing-calendars"></a>列出行事曆

首先, 讓我們來檢查如何列舉已在行事曆應用程式中註冊的行事曆。 若要這麼做, 我們可以具`CursorLoader`現化。 在 Android 3.0 (API 11) 中引進`CursorLoader` , 是`ContentProvider`使用的慣用方式。 我們至少需要指定行事曆的內容 Uri 和我們想要傳回的資料行;此資料行規格稱為「_投射_」。

`CursorLoader.LoadInBackground`呼叫方法可讓我們查詢內容提供者的資料, 例如行事曆提供者。
`LoadInBackground`執行實際的載入作業, 並`Cursor`傳回具有查詢結果的。

協助我們同時指定內容`Uri`和投影。 `CalendarContract` 若要取得查詢`Uri`行事曆的內容, 我們可以直接使用`CalendarContract.Calendars.ContentUri`屬性, 如下所示:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

`CalendarContract`使用來指定我們想要的行事歷數據行, 也一樣簡單。 我們只會將類別中`CalendarContract.Calendars.InterfaceConsts`的欄位新增至陣列。 例如, 下列程式碼包括行事曆的識別碼、顯示名稱和帳戶名稱:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id`如果您`SimpleCursorAdapter`使用將資料系結至 UI, 請務必包含, 如我們稍後所見。 當內容 Uri 和投射備妥時, 我們會具`CursorLoader`現化並`CursorLoader.LoadInBackground`呼叫方法, 以傳回具有行事歷數據的游標, 如下所示:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

這個範例的 UI 包含`ListView`, 清單中的每個專案都代表一個行事曆。 下列 XML 顯示的標記包含`ListView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

此外, 我們還需要指定每個清單專案的 UI, 我們會將它們放在不同的 XML 檔案中, 如下所示:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

從這裡開始, 這只是一般的 Android 程式碼, 可將資料從游標處系結至 UI。 我們會使用`SimpleCursorAdapter` , 如下所示:

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

在上述程式碼中, 介面卡會採用`sourceColumns`陣列中所指定的資料行, 並將其寫入`targetResources`陣列中每個行事曆專案之陣列中的使用者介面元素。 此處使用的活動是的子類別`ListActivity`, 它`ListAdapter`包含我們設定介面卡的屬性。

以下是顯示最終結果的螢幕擷取畫面, 其中的行事曆資訊顯示于`ListView`:

[![在模擬器中執行的 CalendarDemo, 顯示兩個行事曆專案](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>列出行事曆事件

接下來, 我們來看一下如何列舉指定行事曆的事件。
在上述範例中建立時, 我們會在使用者選取其中一個行事歷時, 顯示事件清單。 因此, 我們需要處理先前程式碼中的專案選取:

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

在此程式碼中, 我們會建立一個意圖來開啟類型`EventListActivity`的活動, 並在意圖中傳遞行事曆的識別碼。 我們需要識別碼來知道要查詢事件的行事曆。 `EventListActivity`在的`OnCreate`方法中, 我們可以從`Intent`取得識別碼, 如下所示:

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

現在讓我們來查詢此行事曆識別碼的事件。 查詢事件的程式類似于我們稍早查詢行事曆清單的方式, 只有這次我們會使用`CalendarContract.Events`類別。 下列程式碼會建立用來抓取事件的查詢:

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

在此程式碼中, 我們會先`Uri` `CalendarContract.Events.ContentUri`從屬性取得事件的內容。 然後, 我們會在 eventsProjection 陣列中指定要取得的事件資料行。
最後, 我們會`CursorLoader`使用此資訊來具現化, 並呼叫載入器的`Cursor` `LoadInBackground`方法, 以傳回包含事件資料的。

若要在 UI 中顯示事件資料, 我們可以使用標記和程式碼, 就像我們之前所做的一樣, 才會顯示行事曆清單。 同樣地, 我們`SimpleCursorAdapter`會使用將資料系結`ListView`至, 如下列程式碼所示:

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

這段程式碼與我們稍早用來顯示行事曆清單的程式碼之間的主要差異, 就`ViewBinder`是使用, 這會在行上設定:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

類別`ViewBinder`可讓我們進一步控制將值系結至 views 的方式。 在此情況下, 我們會使用它來將事件開始時間從毫秒轉換為日期字串, 如下列範例所示:

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

這會顯示事件清單, 如下所示:

[![顯示三個行事曆事件之範例應用程式的螢幕擷取畫面](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>加入行事曆事件

我們已瞭解如何讀取行事歷數據。 現在讓我們來看一下如何將事件新增至行事曆。 若要這麼做, 請務必包含先前所`android.permission.WRITE_CALENDAR`述的許可權。 若要將事件新增至行事曆, 我們將:

1. `ContentValues`建立實例。
1. 使用`CalendarContract.Events.InterfaceConsts`類別中的索引鍵來`ContentValues`填入實例。
1. 設定事件開始和結束時間的時區。
1. `ContentResolver`使用將事件資料插入行事曆。


下列程式碼說明這些步驟:

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

請注意, 如果我們未設定時區, 則會擲回類型`Java.Lang.IllegalArgumentException`的例外狀況。 因為在 epoch 之後, 事件時間值必須以毫碼錶示, 所以我們`GetDateTimeMS`會建立方法`EventListActivity`(在中為), 以將日期規格轉換成毫秒格式:

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

如果我們在 [事件清單] UI 中加入一個按鈕, 並在按鈕的 click 事件處理常式中執行上述程式碼, 該事件就會新增至行事曆並在清單中更新, 如下所示:

[![範例應用程式的螢幕擷取畫面, 其中包含行事曆事件, 後面接著新增範例事件按鈕](calendar-images/13.png)](calendar-images/13.png#lightbox)

如果我們開啟行事曆應用程式, 就會看到事件也寫在那裡:

[![顯示所選行事曆事件之行事曆應用程式的螢幕擷取畫面](calendar-images/14.png)](calendar-images/14.png#lightbox)

如您所見, Android 可讓您強大且輕鬆地存取, 以取得和保存行事歷數據, 讓應用程式能夠順暢地整合行事曆功能。


## <a name="related-links"></a>相關連結

- [行事曆示範 (範例)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [霜淇淋三明治簡介](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 平臺](https://developer.android.com/sdk/android-4.0.html)
