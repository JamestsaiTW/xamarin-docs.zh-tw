---
title: Xamarin 中的 CoreML 簡介
description: 本檔說明 CoreML, 這可讓您在 iOS 上進行機器學習。 本檔討論如何開始使用 CoreML, 以及如何將它與願景架構搭配使用。
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/30/2017
ms.openlocfilehash: c2092cd9e7beb233c9478869ebff91d85b5b30c0
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649610"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Xamarin 中的 CoreML 簡介

CoreML 將機器學習服務帶入 iOS –應用程式可以利用經過訓練的機器學習模型來執行各種工作, 從問題解決到影像辨識。

本簡介涵蓋下列各項:

- [使用 CoreML 消費者入門](#coreml)
- [搭配使用 CoreML 與視覺架構](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>使用 CoreML 消費者入門

這些步驟說明如何將 CoreML 新增至 iOS 專案。 如需實用範例, 請參閱[Mars Habitat Pricer 範例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/)。

![Mars Habitat 價格預測器範例螢幕擷取畫面](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1.將 CoreML 模型加入至專案

將 CoreML 模型 (副檔名為**mlmodel**的檔案) 新增至專案的**Resources**目錄。 

在模型檔案的屬性中, 其**組建動作**會設定為**CoreMLModel**。 這表示在建立應用程式時, 它會編譯成**mlmodelc**檔案。

### <a name="2-load-the-model"></a>2.載入模型

使用`MLModel.Create`靜態方法載入模型:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3.設定參數

模型參數會使用所執行的容器類別`IMLFeatureProvider`來傳入和傳出。

功能提供者類別的行為就像是字串`MLFeatureValue`和的字典, 其中每個功能值可以是簡單字串或數位、陣列或資料, 或包含影像的圖元緩衝區。

單一值功能提供者的程式碼如下所示:

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

使用像這樣的類別, 可以透過 CoreML 瞭解的方式來提供輸入參數。 功能的名稱 (例如`myParam`程式碼範例中的) 必須符合模型預期的內容。

### <a name="4-run-the-model"></a>4.執行模型

使用模型時, 會要求將功能提供者具現化並設定參數, 然後`GetPrediction`呼叫方法:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5.將結果解壓縮

預測結果`outFeatures`也是的`IMLFeatureProvider`實例; 您可以使用`GetFeatureValue`和每個輸出參數的`theResult`名稱 (例如) 來存取輸出值, 如下列範例所示:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>搭配使用 CoreML 與視覺架構

CoreML 也可以搭配使用視覺架構來執行影像上的作業, 例如圖形辨識、物件識別和其他工作。

下列步驟說明如何在[CoreMLVision 範例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)中一起使用 CoreML 和願景。 此範例會結合來自視覺架構的[矩形](~/ios/platform/introduction-to-ios11/vision.md#rectangles)辨識與_MNINSTClassifier_ CoreML 模型, 以識別相片中的手寫數位。

![數位3的影像辨識](coreml-images/vision3.png) ![數位5的影像辨識](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1.建立願景 CoreML 模型

CoreML 模型_MNISTClassifier_會載入, 然後包裝在中`VNCoreMLModel` , 讓模型可供視覺工作使用。 此程式碼也會建立兩個視覺要求: 第一個用於尋找影像中的矩形, 然後用來處理 CoreML 模型的矩形:

```csharp
// Load the ML model
var bundle = NSBundle.MainBundle;
var assetPath = bundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
NSError mlErr, vnErr;
var mlModel = MLModel.Create(assetPath, out mlErr);
var model = VNCoreMLModel.FromMLModel(mlModel, out vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(model, HandleClassification);
```

類別仍然需要`HandleRectangles`實作為願景要求的`HandleClassification`和方法, 如下面的步驟3和4所示。

### <a name="2-start-the-vision-processing"></a>2.開始視覺處理

下列程式碼會開始處理要求。 在**CoreMLVision**範例中, 此程式碼會在使用者選取影像之後執行:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

此處理程式會`ciImage`將傳遞至在`VNDetectRectanglesRequest`步驟1中建立的願景架構。

### <a name="3-handle-the-results-of-vision-processing"></a>3.處理視覺處理的結果

完成矩形偵測之後, 它會執行`HandleRectangles`方法, 這會裁剪影像以解壓縮第一個矩形, 將矩形影像轉換成灰階, 並將它傳遞至 CoreML 模型以進行分類。

傳遞`request`至這個方法的參數包含願景要求的詳細資料, 並`GetResults<VNRectangleObservation>()`使用方法, 它會傳回在影像中找到的矩形清單。 第一個矩形`observations[0]`會解壓縮並傳遞至 CoreML 模型:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] {ClassificationRequest}, out NSError err);
  });
}
```

已在步驟1中初始化, 以使用`HandleClassification`下一個步驟中定義的方法。 `ClassificationRequest`

### <a name="4-handle-the-coreml"></a>4.處理 CoreML

傳遞`request`至這個方法的參數包含 CoreML 要求的詳細資料, 並`GetResults<VNClassificationObservation>()`使用方法, 它會傳回依信心排序的可能結果清單 (最高的信賴度):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  // ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```

## <a name="samples"></a>範例

有三個 CoreML 範例可嘗試:

* [Mars Habitat 價格預測器範例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/)具有簡單的數值輸入和輸出。

* 「[願景 & CoreML 範例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)會接受影像參數, 並使用視覺架構來識別影像中的正方形區域, 而這些方塊會傳遞給可辨識單一數位的 CoreML 模型。

* 最後, [CoreML 影像辨識範例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition)會使用 CoreML 來識別相片中的功能。 根據預設, 它會使用較小的**SqueezeNet**模型 (5mb), 但它已被寫入, 讓您可以下載並併入較大的**VGG16**模型 (553MB)。 如需詳細資訊, 請參閱[範例的讀我檔案](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md)。

## <a name="related-links"></a>相關連結

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML 範例 (Mars Habitat) (範例)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/)
- [CoreML 和願景 (數位辨識) (範例)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)
- [CoreML 影像辨識 (範例)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition)
- [使用 Azure 自訂視覺 CoreML (範例)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlazuremodel)
- [CoreML 簡介 (WWDC) (影片)](https://developer.apple.com/videos/play/wwdc2017/703/)
