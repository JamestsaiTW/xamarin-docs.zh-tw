---
title: Xamarin.Android 錯誤矩陣
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: f3721ad661f4b817375b0d625c9b5cc293e6d44c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945301"
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin.Android 錯誤矩陣

## <a name="errors-reference"></a>錯誤參考

本文件提供各種錯誤碼 Xamarin 的一些資訊。

|分類|描述|
|--- |--- |
|XA0xxx|mandroid 錯誤|
|XA1xxx|檔案複製 / 符號連結 （相關的專案） 錯誤|
|XA2xxx|連結器錯誤|
|XA3xxx|AOT 錯誤|
|XA4xxx|程式碼產生錯誤|
|XA5xxx|GCC 和工具鏈錯誤|
|XA6xxx|mandroid 內部工具錯誤|
|XA7xxx|保留|
|XA8xxx|保留|
|XA9xxx|授權錯誤|


## <a name="error-codes"></a>錯誤碼

### <a name="xa0xxx-errors"></a>XA0xxx 錯誤

|錯誤碼|描述|
|--- |--- |
|XA0000|未預期的錯誤-請填寫[bug 報表](https://github.com/xamarin/xamarin-android/issues/new)。|
|XA0001|'-devname 提供不執行任何裝置特定動作。|
|XA0002|無法剖析環境變數 '{0}'。|
|XA0003|應用程式名稱 '{0}.exe' je v konfliktu SDK 或產品組件 (.dll) 名稱。|
|XA0004|新的 refcounting 邏輯需要太啟用 sgen。|
|XA0005|輸出目錄 '{0}' 不存在。|
|XA0006|沒有任何提供平台 '{0}'，使用-平台 = 指定 SDK 的平台|
|XA0007|根組件 '{0}' 不存在。|
|XA0008|您應該提供一個根組件只。|
|XA0009|載入組件時發生錯誤： {0}。|
|XA0010|無法剖析命令列引數： {0}。|
|XA0011|{0} 較新的執行階段建置 ({1}) 高於 MonoTouch 支援。|
|XA0012|若要完成不完整的資料提供 '{0}'。|
|XA0013|程式碼剖析支援需要太啟用 sgen。|
|XA0014|iOS{0}不支援目標 ARMv6 的建置應用程式。|
|XA0020|無法判斷 mandroid 路徑。|
|XA0100|EmbeddedNativeLibrary '{0}' 在 Android 應用程式專案中無效。 請改用 AndroidNativeLibrary。|

### <a name="xa1xxx-errors"></a>XA1xxx 錯誤

|錯誤碼|描述|
|--- |--- |
|XA1001|找不到應用程式在指定的目錄。|
|XA1002|無法建立符號連結，已複製檔案。|
|XA1003|無法終止應用程式 '{0}'。 您可能必須手動終止的應用程式。|
|XA1004|無法取得已安裝的應用程式清單。|
|XA1005|無法終止應用程式 '{0}'上的裝置{1}': {2}。 您可能必須手動終止的應用程式。|
|XA1006|無法安裝應用程式 '{0}'上的裝置{1}': {2}。|
|XA1007|無法啟動應用程式 '{0}'上的裝置{1}': {2}。 您仍然可以在其上的點選以手動方式啟動應用程式。|
|XA1008|無法啟動模擬器： {0}。|
|XA1009|無法複製組件 '{0}'到'{1}': {2}。|
|XA1010|無法載入組件 '{0}': {1}。|
|XA1011|無法加入遺漏的資源檔: '{0}'。|
|XA1101|無法啟動應用程式。|
|XA1102|無法附加至應用程式 （以終止為止）： {0}。|
|XA1103|無法中斷連結。|
|XA1104|無法傳送封包： {0}。|
|XA1105|未預期的回應類型。|
|XA1106|無法在裝置上取得應用程式的清單：要求已逾時。|
|XA1107|應用程式無法啟動。|
|XA1201|無法載入模擬器： {0}。|
|XA1301|原生程式庫 '{0}' ({1}) 已被忽略，因為它不符合目前的組建 architecture(s) ({2})。|

### <a name="xa2xxx-errors"></a>XA2xxx 錯誤

|錯誤碼|描述|
|--- |--- |
|XA2001|無法將組件的連結。|
|XA2002|無法解析參考： {0}。|
|XA2003|選項 '{0}' 將被忽略，因為連結已停用。|
|XA2004|額外的連結器定義檔 '{0}' 找不到。|
|XA2005|定義從 '{0}' 無法剖析。|
|XA2006|中繼資料項目參考 '{0}' (定義於 '{1}') 從'{2}' 無法解析。|

### <a name="xa3xxx-errors"></a>XA3xxx 錯誤

這些是 AOT 錯誤。

|錯誤碼|描述|
|--- |--- |
|XA3001|無法 AOT 的組件 '{0}'。|
|XA3002|AOT 限制：方法 '{0}' 必須是靜態，因為它附有 [MonoPInvokeCallback]。|
|XA3003|衝突-偵錯和-llvm 選項。 軟偵錯已停用。|


### <a name="xa4xxx-errors"></a>XA4xxx 錯誤

這些是程式碼產生錯誤。

|錯誤碼|描述|
|--- |--- |
|XA4001|找不到 expansed 主要的範本 '{0}'。|
|XA4101|在註冊機構的版本無法建立類型的簽章 '{0}'。|
|XA4102|在註冊機構找到無效的類型 '{0}'中方法簽章'{2}'。 使用 '{1}' 改為。|
|XA4103|在註冊機構找到無效的類型 '{0}'中方法簽章'{2}':型別會實作 INativeObject，但並沒有會採用兩個建構函式 (IntPtr，bool) 引數。|
|XA4104|在註冊機構無法封送處理的傳回值為類型 '{0}'中方法簽章'{1}'。|
|XA4105|在註冊機構無法封送處理的型別參數 '{0}'中方法簽章'{1}'。|
|XA4106|在註冊機構無法封送處理結構的傳回值 '{0}'中方法簽章'{1}'。|
|XA4107|在註冊機構無法封送處理的型別參數 '{0}'中方法簽章'{1}'。|
|XA4108|在註冊機構無法取得 managed 類型的 ObjectiveC 類型 '{0}'。|
|XA4109|無法編譯產生的註冊機構的程式碼。 請提出[bug 報表](http://bugzilla.xamarin.com)。|
|XA4110|在註冊機構無法封送處理類型的 out 參數 '{0}'中方法簽章'{1}'。|
|XA4111|在註冊機構的版本無法建立類型的簽章 '{0}'中方法'{1}'。|
|XA4200|只產生 ACW 的 'claas' 類型。|
|XA4201|無法判斷類型的 JNI 名稱{0}。|
|XA4203|指定的型別名稱必須是完整名稱。|
|XA4204|無法解析介面型別 '{0}'。 您是否遺漏了組件參考?|
|XA4205|[ExportField] 僅適用於具有 0 個參數的方法。|
|XA4206|[匯出] 不能是泛型型別。|
|XA4207|[ExportField] 不能是泛型型別。|
|XA4208|[Java.Interop.ExportFieldAttribute] 不能傳回 void 的方法上。|
|XA4209|無法建立 JavaTypeInfo 類別：{0}由於{1}。|
|XA4210|您需要將參考加入 Mono.Android.Export.dll，當您使用 exportattribute 標記或 ExportFieldAttribute。|
|XA4211|AndroidManifest.xml //uses-sdk/@android:targetSdkVersion '{0}'小於 $(TargetFrameworkVersion)'{1}'。 使用 API-{1} ACW 編譯。|


### <a name="xa5xxx-errors"></a>XA5xxx 錯誤

|錯誤碼|描述|
|--- |--- |
|XA5101|遺漏 '{0}' 編譯器。 請安裝 Android NDK。|
|XA5102|從組件轉換成原生程式碼失敗。 請提出[bug 報表](http://bugzilla.xamarin.com)。|
|XA5103|無法編譯檔案 '{0}'。 請提出[bug 報表](http://bugzilla.xamarin.com)。|
|XA5201|原生連結失敗。 請檢閱使用者提供給 gcc 的旗標： {0}|
|XA5202|原生連結失敗。 請檢閱建置記錄檔。|
|XA5303|原生連結警告： {0}。|
|XA5300|找不到 android SDK，或未完整安裝。|
|XA5301|遺漏 '移除' 工具。 請安裝 Xcode '命令列工具 」 元件。|
|XA5302|遺漏 'dsymutil' 工具。 請安裝 Xcode '命令列工具 」 元件。|
|XA5203|無法產生偵錯符號 （dSYM 目錄）。 請檢閱建置記錄檔。|
|XA5204|無法刪除最終二進位檔。 請檢閱建置記錄檔。|
|XA5205|遺漏 'aapt' 工具。 請安裝 Android SDK 建置工具套件。|
|XA5206|{0}. Android 資源目錄{1}不存在。|
|XA5207|{0}. Java 程式庫檔案{1}不存在。|
|XA5208|下載失敗。 請下載{0}並將它放到{1}目錄。|
|XA5209|解壓縮失敗。 請下載{0}並將其解壓縮至{1}目錄。|
|XA5210|{0}. 原生程式庫檔案{1}不存在。|

### <a name="xa6xxx-errors"></a>XA6xxx 錯誤

|錯誤碼|描述|
|--- |--- |
|XA6001|Cecil 執行版本不支援移除的組件。|
|XA6002|無法移除組件 '{0}'。|
|XA6003|UnauthorizedAccessException 訊息。|

### <a name="xa9xxx-errors"></a>XA9xxx 錯誤

這些錯誤碼是授權和啟用的錯誤。


#### <a name="xa9000"></a>XA9000

 **原因：** 授權已過期

 **檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|WARNING|WARNING|WARNING|WARNING|WARNING|


#### <a name="xa9001"></a>XA9001

 **原因：** 試用版已過期

 **檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|WARNING|WARNING|WARNING|WARNING|WARNING|



#### <a name="xa9002"></a>XA9002

 **原因：** AndroidJavaSource

 **檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|確定|確定|確定|確定|

 **原因：** AndroidJavaLibrary

 **檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|確定|確定|確定|確定|

 **如果繫結組件有內嵌.jar，這會攔截在封裝期間，不是建置時間。**

 **原因：** AndroidNativeLibrary

 **檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|確定|確定|確定|確定|


#### <a name="xa9003"></a>XA9003

 **原因：** System.Runtime.Serialization

 **檢查期間：** 套件

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|確定|確定|確定|

 **原因：** System.ServiceModel.Web

 **檢查期間：** 套件

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|確定|確定|確定|

 **原因：** Mono.Data.Tds

 **檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|確定|確定|確定|

 **這是由 System.Data.dll，允許的參考**

 **原因：** Mono.Android.Export

 **檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|確定|確定|確定|確定|



#### <a name="xa9004"></a>XA9004

 **原因：** -程式碼剖析

 **檢查期間：** 套件

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|確定|確定|確定|



#### <a name="xa9005"></a>XA9005

 **原因：** 大小上限 (32 kb)。

 **檢查期間：** 套件

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|確定|-|-|-|



#### <a name="xa9006"></a>XA9006

 **原因：** System.Data.SqlClient 命名空間。

 **檢查期間：** 套件

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|確定|確定|確定|


#### <a name="xa9008"></a>XA9008

 **原因：** 從命令列建置。

 **檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|確定|確定|確定|


#### <a name="xa9009"></a>XA9009

 **原因：** 遺漏的序號。

 **檢查期間：** 無法測試又複雜

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9010"></a>XA9010

 **原因：** 無效的產品識別碼。

 **檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|

相當於 XA9018。



#### <a name="xa9011"></a>XA9011

 **原因：** 無法更新授權檔案 （以新的檔案格式）。

 **檢查期間：** 啟用

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|

#### <a name="xa9012"></a>XA9012

 **原因：** 沒有網際網路

 **檢查期間：** 啟用

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9013"></a>XA9013

 **原因：** 未知的錯誤

 **檢查期間：** 啟用

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9014"></a>XA9014

 **原因：** 無效的啟用代碼

 **檢查期間：** 啟用

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9017"></a>XA9017

 **原因：** 啟用伺服器不會傳回有效的授權。

 **檢查期間：** 啟用

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### <a name="xa9018"></a>XA9018

**原因：** 無效的授權

**檢查期間：** 組建

|入門|獨立製作|Business(Trial)|商務|企業|
|--- |--- |--- |--- |--- |
|-|-|ERROR|-|-|

