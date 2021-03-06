---
title: ContentProviders 簡介
description: Android 作業系統使用內容提供者來加速存取共用資料, 例如媒體檔案、連絡人和行事曆資訊。 本文介紹 ContentProvider 類別, 並提供兩個如何使用它的範例。
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 2533ad80571e2c8fe94cb4a2dcb0ec0ff0dd68cb
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643592"
---
# <a name="intro-to-contentproviders"></a>ContentProviders 簡介

_Android 作業系統使用內容提供者來加速存取共用資料, 例如媒體檔案、連絡人和行事曆資訊。本文介紹 ContentProvider 類別, 並提供兩個如何使用它的範例。_


## <a name="content-providers-overview"></a>內容提供者總覽

*ContentProvider*會封裝資料存放庫, 並提供 API 來存取它。 提供者是 Android 應用程式的一部分, 通常也會提供 UI 來顯示/管理資料。 使用內容提供者的主要優點是讓其他應用程式可以使用提供者用戶端物件 (稱為*ContentResolver*) 輕鬆地存取封裝的資料。 內容提供者和內容解析程式共同提供了一致的應用程式間 API, 可讓您輕鬆建立及取用資料存取。 任何應用程式都可以選擇`ContentProviders`用來在內部管理資料, 並將它公開給其他應用程式。

`ContentProvider`您的應用程式也需要提供自訂搜尋建議, 或者, 如果您想要提供從應用程式複製複雜資料以貼到其他應用程式的功能。 本檔說明如何使用 Xamarin 來存取`ContentProviders`和建立。

本節的結構如下所示:

- **運作方式**概述設計的`ContentProvider`功能及其運作方式。 &ndash;

- **使用內容提供者**&ndash;存取連絡人清單的範例。

- **使用 ContentProvider 共用資料**在同`ContentProvider`一個應用程式中撰寫和使用。 &ndash;

`ContentProviders`而在其資料上操作的游標通常是用來填入 Listview。 如需如何使用這些類別的詳細資訊, 請參閱[listview 和介面卡指南](~/android/user-interface/layouts/list-view/index.md)。

`ContentProviders`由 Android (或其他應用程式) 公開, 是在您的應用程式中包含來自其他來源之資料的簡單方式。 它們可讓您從應用程式記憶體取和呈現資料, 例如連絡人清單、相片或行事曆事件, 並讓使用者與該資料互動。

「 `ContentProviders`自訂」是一種方便的方式, 可將您的資料封裝在您自己的應用程式內使用, 或供其他應用程式使用 (包括自訂搜尋和複製/貼上等特殊用途)。

本節中的主題提供一些使用和撰寫`ContentProvider`程式碼的簡單範例。



## <a name="related-links"></a>相關連結

- [ContactsAdapter 示範 (範例)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [SimpleContentProvider (範例)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [內容提供者開發人員指南](https://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider 類別參考](xref:Android.Content.ContentProvider)
- [ContentResolver 類別參考](xref:Android.Content.ContentResolver)
- [ListView 類別參考](xref:Android.Widget.ListView)
- [CursorAdapter 類別參考](xref:Android.Widget.CursorAdapter)
- [UriMatcher 類別參考](xref:Android.Content.UriMatcher)
- [Android.Provider](xref:Android.Provider)
- [ContactsContract 類別參考](xref:Android.Provider.ContactsContract)
