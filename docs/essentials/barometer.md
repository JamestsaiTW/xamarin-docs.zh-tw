---
title: 'Xamarin.Essentials: Barometer'
description: Xamarin.Essentials 中的 Barometer 類別可讓您監視裝置的氣壓計感應器，其會測量壓力。
ms.assetid: DA4F968A-D988-41F5-8745-1BEE693660A1
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 342ae1b64fefebaa4b3fa82e9f48c6e9a58d4751
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899040"
---
# <a name="xamarinessentials-barometer"></a>Xamarin.Essentials: Barometer

**Barometer** 類別可讓您監視裝置的氣壓計感應器，其會測量壓力。

## <a name="get-started"></a>開始使用

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-barometer"></a>使用 Barometer

在類別中新增對 Xamarin.Essentials 的參考：

```csharp
using Xamarin.Essentials;
```

Barometer 功能的運作方式是呼叫 `Start` 與 `Stop` 方法，以觀察氣壓計壓力的變更 (讀數單位為百帕)。 所有變化都會透過 `ReadingChanged` 事件傳回。 以下是範例使用方式：

```csharp

public class BarometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public BarometerTest()
    {
        // Register for reading changes.
        Barometer.ReadingChanged += Barometer_ReadingChanged;
    }

    void Barometer_ReadingChanged(object sender, BarometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process Pressure
        Console.WriteLine($"Reading: Pressure: {data.PressureInHectopascals} hectopascals");
    }

    public void ToggleBarometer()
    {
        try
        {
            if (Barometer.IsMonitoring)
              Barometer.Stop();
            else
              Barometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="platform-implementation-specifics"></a>平台實作特性

# <a name="androidtabandroid"></a>[Android](#tab/android)

沒有平台特定實作詳細資料。

# <a name="iostabios"></a>[iOS](#tab/ios)

此 API 使用 [CMAltimeter](https://developer.apple.com/documentation/coremotion/cmaltimeter#//apple_ref/occ/cl/CMAltimeter) 來監視壓力變更，這是 iPhone 6 和更新的裝置上加入的硬體功能。 不支援高度計的裝置將會擲回 `FeatureNotSupportedException`。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

沒有平台特定實作詳細資料。

-----

## <a name="api"></a>API

- [Barometer 原始程式碼](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Barometer)
- [Barometer API 文件](xref:Xamarin.Essentials.Barometer)
