---
title: 適用于 Xamarin 的 XAML 熱重載
description: 在執行中的應用程式上立即重載 XAML 檔案的變更, 讓您不需要在每次 XAML 變更後建立您的 Xamarin. Forms 專案。
ms.prod: xamarin
ms.assetid: E220F054-32EE-424C-A7E5-6156BE271519
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 08/13/2019
ms.openlocfilehash: 4f6a0b45d37252c141b2741dd0b37a980c958a51
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2019
ms.locfileid: "68984530"
---
# <a name="xaml-hot-reload-for-xamarinforms-preview"></a>適用于 Xamarin 的 XAML 熱重載 (預覽)

![預覽功能](~/media/shared/preview.png)

XAML 熱重載會插入您現有的工作流程, 以提高您的生產力並節省您的時間。 如果沒有 XAML 熱重載, 您就必須在每次想要查看 XAML 變更時, 建立並部署您的應用程式。 使用熱重載時, 當您儲存 XAML 檔案時, 變更會即時反映在執行中的應用程式中。 此外, 您的導覽狀態和資料將會維護, 讓您快速反復查看 UI, 而不會遺失您在應用程式中的位置。 因此, 使用 XAML 熱重載, 您會花較少的時間重建和部署應用程式, 以驗證 UI 變更。

## <a name="system-requirements"></a>系統需求

| IDE/架構 | 需要的版本 |
|------|------------------|
|Visual Studio 2019 | 16.3 或更高版本
Visual Studio 2019 for Mac | 8.3 或更高版本
Xamarin.Forms | 4.1 或更高版本

## <a name="use-xaml-hot-reload-for-xamarinforms"></a>針對 Xamarin 使用 XAML 熱重載

不需要進行其他安裝或設定, 就能使用 XAML 熱重載。 它內建在 Visual Studio 中, 而且可以在 IDE 設定中啟用。 啟用之後, 您就可以在模擬器、模擬器或實體裝置上對應用程式進行偵測, 以開始使用 XAML 熱重載。

在 Windows 上, 您可以在 [**工具** > ] [**選項** > ] [**xamarin**  > **熱重載**] 中勾選 [**啟用 Xamarin 熱**重載] 核取方塊, 以啟用 XAML 熱重載。

在 Mac 上, 您可以勾選 [在**Visual Studio**  > **喜好** > 設定**專案** > ] [**xamarin 熱重載**] 中的 [**啟用 Xamarin 熱**重載] 核取方塊, 以啟用 XAML 熱重載。

## <a name="resilient-reloading"></a>復原重載

如果您進行的變更是 XAML 熱重載無法重載, 則會使用 IntelliSense 顯示錯誤訊息。 這些變更 (稱為「強制編輯」) 包含不正確的 XAML, 或將控制項連接到不存在的事件處理常式。 即使使用 [編輯], 您仍可繼續重載而不重新開機應用程式-在 XAML 檔案中的其他位置進行其他變更, 然後點擊 [儲存]。 不會重載 [強制編輯], 但會繼續套用您的其他變更。

## <a name="known-limitations"></a>已知限制

- 您無法在 XAML 熱重載會話期間加入、移除或重新命名檔案或 NuGet 套件。 如果您新增或移除檔案或 NuGet 套件, 請重建並重新部署您的應用程式, 以繼續使用 XAML 熱重載。
- 將連結器設定為 [**無] 連結**, 以獲得最佳體驗。 「**僅限連結 SDK** 」設定會在大部分的時間內運作, 但在某些情況下可能會失敗。
- 在實體 iPhone 上進行偵錯工具時, 解譯器必須使用 XAML 熱重載。 將 **--解釋**器新增至 iOS 組建設定中的 [**其他 mtouch 引數**] 欄位, 以使用 XAML 熱重載。
- 使用其`x:Name`值將控制項指派給另一個欄位或屬性所建立的任何參考都不會重載。
- 在**AppShell**中更新 Shell 應用程式的視覺階層, 可能會導致維護應用程式狀態的問題。 重建應用程式以繼續重載。
- XAML 熱重載無法重載C#程式碼, 包括事件處理常式、自訂控制項、分頁程式碼後置和其他類別。

## <a name="migrate-from-the-private-preview"></a>從私人預覽遷移

如果您是個人預覽版的一部分, 當 Visual Studio 更新時, 您的 XAML 熱重載延伸模組將會自動更新。 如果您選擇不更新 Visual Studio, 您可以繼續使用目前版本的 XAML 熱重載, 但不會透過私人預覽延伸模組摘要收到任何進一步的更新。

## <a name="troubleshooting"></a>疑難排解

- 如果 XAML 熱重載無法初始化:
  - 更新您的 Xamarin. 表單版本。
  - 請確定您是在最新版本的 IDE 上。
  - 將您的 Android 或 iOS 連結器設定設定為 [**不要連結**] 專案的組建設定。
- 如果儲存 XAML 檔案時沒有發生任何事, 請確定已在 IDE 中啟用熱重載。
- 如果您要在實體 iPhone 上進行偵錯工具, 而您的應用程式變得沒有回應, 請檢查解譯器是否已啟用。 若要將它開啟, 請將 **--解釋**器新增至 iOS 組建設定中的 [**其他 mtouch 引數**] 欄位。

若要回報錯誤, 請使用 Windows 上的 [  > 說明] [**傳送意見** > 反應] [回報**問題**] 功能表  > 上的 [意見反應] 工具, 以及 Mac 上的 [說明] [回報**問題**] 功能表。
