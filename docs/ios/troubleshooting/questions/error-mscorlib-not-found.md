---
title: 執行階段錯誤：找不到或無法載入組件 mscorlib.dll
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 84bb6815c19bcacb4a9d1bddc44d340d51199c32
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421979"
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>執行階段錯誤：找不到或無法載入組件 mscorlib.dll

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

就會發生此問題時*隱藏*`.monotouch-32`並`.monotouch-64`資料夾中遺漏的`.xcarchive`來簽署 / IPA 建立、 執行階段錯誤。

