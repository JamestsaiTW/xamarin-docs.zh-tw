---
title: Google 授權服務
ms.prod: xamarin
ms.assetid: E96BDCC3-454A-A797-5819-905E2BB1AC41
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 12/20/2017
ms.openlocfilehash: eedfcfe2ed274ddf541addec67e66250deab7899
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114622"
---
# <a name="google-licensing-services"></a>Google 授權服務

在有 Google Play 之前，Android 應用程式倚賴 Google Market 所提供的舊版「複製防護」(Copy Protection)，來確保只有已取得授權的使用者才能在其裝置上執行應用程式。 由於「複製防護」機制有所限制，因此使得它無法成為理想的應用程式保護解決方案。

「Google 授權」是用來取代這個舊版的「複製防護」機制。
「Google 授權」是一個具彈性、安全的網路型服務，Android 應用程式可查詢此服務，來判斷應用程式是否已取得在指定裝置上執行的授權。

「Google 授權」的彈性在於 Android 應用程式能夠完全控制何時檢查授權、多久檢查一次授權，以及如何處理來自授權伺服器的回應。

「Google 授權」的安全性在於每個回應都會以僅在 Google Play 與應用程式之間共用的 RSA 金鑰組簽署。 Google Play 會為開發人員提供一個公開金鑰，此金鑰內嵌在 Android 應用程式並可用來驗證回應。 Google Play 伺服器會將私密金鑰保留在內部。

已實作「Google 授權」的應用程式會向裝置上 Google Play 應用程式所主控的服務提出要求。 Google Play 會接著將此要求傳送給「Google 授權」伺服器，由該伺服器回應授權狀態： 

[![授權伺服器工作流程圖](google-licensing-services-images/gp-licensing-service-overview.png)](google-licensing-services-images/gp-licensing-service-overview.png#lightbox)

上圖說明以下工作流程： 

-   應用程式提供套件名稱、用來驗證伺服器回應的 *nonce* (密碼編譯驗證器)，以及能夠以非同步方式處理回應的回呼。 

-   Google Play 提供資訊 (例如 Google 帳戶) 和裝置本身 (例如 IMSI 號碼)。 

「Google 授權」服務也是 APK 擴充檔 (本文件稍後會提供說明) 的主要元件。 APK 擴充檔會利用「Google 授權」服務來取得將下載之擴充檔的 URL。


## <a name="requirements"></a>需求

不是透過 Google Play 購買的應用程式將無法從「Google 授權」服務獲得任何好處。 如果裝置上未安裝 Google Play，則使用「授權服務」的應用程式將仍可在該裝置上正常運作。

Google Play 必須能夠存取網際網路才可運作。 如果裝置無法存取「Google Play 授權」伺服器，應用程式將可快取授權來因應這類情況。

免費應用程式只有在使用 APK 擴充檔時，才需要「Google 授權」。
