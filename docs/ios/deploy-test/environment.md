---
title: Xamarin.iOS 應用程式的執行環境
description: 本文件描述如何設定 Xamarin.iOS 應用程式的暫時性和永久性環境變數。 這些變數可在專案內容中指定，或當成 mtouch 封裝工具的額外引數。
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: 52ae48a0cd911cd90d985b830876ec003d073d2a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119295"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Xamarin.iOS 應用程式的執行環境

「執行環境」是一組環境變數，會影響程式的執行。 環境變數可在專案屬性中暫時設定，或透過將額外引數指定給 mtouch 封裝工具來永久設定。

## <a name="temporary-environment-variables"></a>暫存環境變數

暫存環境變數可在專案的 [屬性]/[選項] 視窗中的 [執行] > [一般] 區段中進行設定。 這些環境變數只會在應用程式使用 Visual Studio for Mac 啟動時才會生效。若應用程式是透過手動點選啟動的，則將不會設定這些環境變數。

## <a name="permanent-environment-variables"></a>永久環境變數

永久環境變數可藉由將額外的引數指定給 mtouch 封裝工具來設定。 這些環境變數會編譯到可執行檔中，即使應用程式不是從 Visual Studio for Mac 啟動也會設定這些環境變數。

## <a name="example"></a>範例

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

