---
title: 'Xamarin.Essentials: MainThread'
description: MainThread 類別可讓應用程式在主執行緒上執行程式碼。
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 7ec1420d87c898f63614eb6d980c28834e980afd
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899001"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

**MainThread** 類別可讓應用程式在主執行緒上執行程式碼，並判斷特定程式碼區塊目前是否在主執行緒上執行。

## <a name="background"></a>背景

大部分的作業系統 (包括 iOS、Android 與通用 Windows 平台) 都為牽涉到使用者介面的程式碼使用單一執行緒處理模型。 需要此此模型才能適當地序列化使用者介面事件，包括按鍵點擊與觸控輸入。 此執行緒通常稱為「主執行緒」或「使用者介面執行緒」或「UI 執行緒」。 此模型的缺點是存取使用者介面元素的所有程式碼都必須在應用程式的主執行緒上執行。 

應用程式有時候需要使用呼叫次要執行緒上之事件處理常式的事件。 (Xamarin.Essentials 類別 [`Accelerometer`](accelerometer.md)、[`Compass`](compass.md)、[`Gyroscope`](gyroscope.md)、[`Magnetometer`](magnetometer.md) 與 [`OrientationSensor`](orientation-sensor.md) 全都可能傳回次要執行緒的資訊 (當以較快的速度使用時)。)若事件處理常式需要存取使用者介面元素，它必須在主執行緒上執行該程式碼。 **MainThread** 類別可讓應用程式在主執行緒上執行此程式碼。

## <a name="get-started"></a>開始使用

[!include[](~/essentials/includes/get-started.md)]

## <a name="running-code-on-the-main-thread"></a>在只執行緒上執行程式碼

在類別中新增對 Xamarin.Essentials 的參考：

```csharp
using Xamarin.Essentials;
```

若要在主執行緒上執行程式碼，請呼叫靜態 `MainThread.BeginInvokeOnMainThread` 方法。 引數是 [`Action`](xref:System.Action) 物件，這是沒有引數與傳回值的方法：

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

也可以針對必須在主執行緒上執行的程式碼定義不同的方法：

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

接著，您可以透過在 `BeginInvokeOnMainThread` 方法中參考它，以主執行緒上執行此方法：

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms 有一個稱為 [`Device.BeginInvokeOnMainThread(Action)`](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) 的方法
> 它會執行與 `MainThread.BeginInvokeOnMainThread(Action)` 相同的功能。 當您在 Xamarin.Forms 應用程式中使用任一方法時，請考慮發出呼叫的程式碼是否有對 Xamarin.Forms 任何其他相依性需求。 若沒有，`MainThread.BeginInvokeOnMainThread(Action)` 很可能是比較好的選項。

## <a name="determining-if-code-is-running-on-the-main-thread"></a>判斷程式碼是否是在主執行緒上執行

`MainThread` 類別也允許應用程式判斷特定程式碼區塊是否在主執行緒上執行。 若呼叫屬性的程式碼是在主執行緒上執行，`IsMainThread` 屬性會傳回 `true`。 程式可以使用此屬性為主執行緒與次要執行緒執行不同的程式碼：

```csharp
if (MainThread.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

您可能想要知道是否應該在呼叫 `BeginInvokeOnMainThread` 之前先檢查程式碼是否在次要執行緒上執行，例如，像這樣：

```csharp
if (MainThread.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

您可能會懷疑若程式碼區塊已在主執行緒上執行，此檢查是否會改善效能。

不過，此檢查並非必要。 `BeginInvokeOnMainThread` 本身的平台實作會檢查呼叫是否針對主執行緒發出。 若在並非必要的情況下呼叫 `BeginInvokeOnMainThread`，會對效能造成極小的影響。

## <a name="api"></a>API

- [MainThread 原始程式碼](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread API 文件](xref:Xamarin.Essentials.MainThread)
