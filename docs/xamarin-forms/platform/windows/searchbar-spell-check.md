---
title: " Windows 上的搜尋列拼寫檢查"
description: 平台特性可讓您使用的功能只可在特定的平台，而不需要實作自訂轉譯器或影響。 本文說明如何使用 Windows 平臺特定的, 讓搜尋列與拼寫檢查引擎互動。
ms.prod: xamarin
ms.assetid: 7974C91F-7502-4DB3-B0E9-C45E943DDA26
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 863748a7af057d2ea3e53719c332cd555ff3b698
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656867"
---
# <a name="searchbar-spell-check-on-windows"></a>Windows 上的搜尋列拼寫檢查

[![下載範例](~/media/shared/download.png) 下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

這個通用 Windows 平臺平臺特定的可讓[`SearchBar`](xref:Xamarin.Forms.SearchBar)與拼寫檢查引擎互動。 它由在 XAML 中設定[ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) ; 附加屬性`boolean`值：

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

或者，它可以取用從 C# 使用 fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>`方法可讓您指定這個特定平台-通用 Windows 平台上只會執行。 [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean))方法，請在[ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)命名空間，將拼字檢查工具，開啟和關閉。 颾魤 ㄛ`SearchBar.SetIsSpellCheckEnabled`方法可用來切換拼字檢查工具，藉由呼叫[ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar}))方法來傳回是否已啟用拼字檢查工具：

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

結果會是該文字方塊中輸入[ `SearchBar` ](xref:Xamarin.Forms.SearchBar)可以拼字檢查，與使用者所表示的拼字不正確：

![SearchBar 拼字檢查平台專屬](searchbar-spell-check-images/searchbar-spellcheck.png "SearchBar 拼字檢查特定平台")

> [!NOTE]
> `SearchBar`類別內[ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)命名空間也有[ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*)並[ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*)可用來啟用和停用的方法拼字檢查`SearchBar`分別。

## <a name="related-links"></a>相關連結

- [PlatformSpecifics （範例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [建立平台特性](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
