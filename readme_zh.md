# QRCoder
[![qrcoder MyGet Build Status](https://www.myget.org/BuildSource/Badge/qrcoder?identifier=10cbdaa5-2dd9-460b-b424-be44e75258ec)](https://www.myget.org/feed/qrcoder/package/nuget/QRCoder)   [![NuGet Badge](https://buildstats.info/nuget/QRCoder)](https://www.nuget.org/packages/QRCoder/)
## 信息

QRCoder是一个简单的库，用C＃.NET编写，使您可以创建QR码。它与其他库没有任何依赖关系，并且可以在NuGet上以.NET Framework和.NET Core PCL版本获得。

随意下载/分叉该项目并将其做得更好！

有关更多信息，请参见:
[**QRCode Wiki**](https://github.com/codebude/QRCoder/wiki) | [Creator's blog (english)](http://en.code-bude.net/2013/10/17/qrcoder-an-open-source-qr-code-generator-implementation-in-csharp/) | [Creator's blog (german)](http://code-bude.net/2013/10/17/qrcoder-eine-open-source-qr-code-implementierung-in-csharp/)

## 法律信息和信用

QRCoder 是 [Raffael Herrmann](http://raffaelherrmann.de) 的项目，于10/2013首次发布。它已获得MIT许可。

* * *

## 安装

Fork此Github存储库，或通过NuGet软件包管理器安装QRCoder。如果要使用NuGet，只需搜索“ QRCoder”或在NuGet软件包管理器控制台中运行以下命令：
```bash
PM> Install-Package QRCoder
```

*注意：NuGet提要仅包含**稳定**版本。如果您不需要最新的版本，则将以下网址之一添加到Visual Studio的NuGet程序包管理器选项的“程序包源”中。*

*NuGet V3 feed URL (Visual Studio 2015+):* `https://www.myget.org/F/qrcoder/api/v3/index.json`

*NuGet V2 feed URL (Visual Studio 2012+):* `https://www.myget.org/F/qrcoder/api/v2`

----

## 用法

您只需要五行代码即可生成和查看您的第一个QR码。

```csharp
QRCodeGenerator qrGenerator = new QRCodeGenerator();
QRCodeData qrCodeData = qrGenerator.CreateQrCode("您输入的文本.", QRCodeGenerator.ECCLevel.Q);
QRCode qrCode = new QRCode(qrCodeData);
Bitmap qrCodeImage = qrCode.GetGraphic(20);
```

### 可选参数和重载

GetGraphics方法具有更多的重载。前两个使您可以设置QR码图形的颜色。一种使用Color-class-types，另一种使用HTML十六进制颜色表示法。

```csharp
//通过使用颜色类类型
Bitmap qrCodeImage = qrCode.GetGraphic(20, Color.DarkRed, Color.PaleGreen, true);

//通过使用HTML十六进制颜色表示法设置颜色
Bitmap qrCodeImage = qrCode.GetGraphic(20, "#000ff0", "#0ff000");
```

另一个重载使您可以在QR码的中心渲染徽标/图像。

```csharp
Bitmap qrCodeImage = qrCode.GetGraphic(20, Color.Black, Color.White, (Bitmap)Bitmap.FromFile("C:\\myimage.png"));
```

还有很多其他选择。因此，随时可以在我们的Wiki上阅读更多内容： [Wiki: How to use QRCoder](https://github.com/codebude/QRCoder/wiki/How-to-use-QRCoder)

### 特殊渲染类型

除了用于以位图格式创建QR码的普通QRCode类（如上例所示）外，还有更多QR码渲染类，每个类都有另一个特殊用途。

* [QRCode<sup>*</sup>](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers#21-qrcode-renderer-in-detail)
* [AsciiQRCode<sup></sup>](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers#22-asciiqrcode-renderer-in-detail)
* [Base64QRCode<sup>*</sup>](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers#23-base64qrcode-renderer-in-detail)
* [BitmapByteQRCode<sup></sup>](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers#24-bitmapbyteqrcode-renderer-in-detail)
* [PngByteQRCode<sup></sup>](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers#25-pngbyteqrcode-renderer-in-detail)
* [PostscriptQRCode<sup>*</sup>](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers#29-postscriptqrcode-renderer-in-detail)
* [SvgQRCode<sup>*</sup>](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers#26-svgqrcode-renderer-in-detail)
* [UnityQRCode<sup>**</sup>](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers#27-unityqrcode-renderer-in-detail)
* [XamlQRCode<sup>*</sup>](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers#28-xamlqrcode-renderer-in-detail)

*（\*）-这些类仅在.NET Framework / .NET Standard版本中可用。如果使用PCL版本（例如，对于通用应用程序），则必须使用BitmapByteQRCode或PngByteQRCode类。*

*（**）-此类托管在一个独立的包（QRCoder.Unity）中。*


有关不同渲染类型的更多信息，请单击上方列表中的一种，或查看以下内容： [Wiki: Advanced usage - QR-Code renderers](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers)

## PayloadGenerator.cs-生成QR码有效载荷

从技术上讲，QR代码只是文本/字符串的视觉表示。尽管如此，大多数QR码阅读器仍可以读取触发不同动作的“特殊” QR码。

例如：WiFi QR码，当被智能手机扫描时，可使智能手机自动加入接入点。

当生成QR码时，通过使用特殊的结构化有效负载字符串来生成此“特殊” QR码。该[PayloadGenerator.cs类](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators)帮助您生成该有效载荷的字符串。例如，要生成WiFi有效负载，您只需要以下一行代码：

```csharp
PayloadGenerator.WiFi wifiPayload = new PayloadGenerator.WiFi("MyWiFi-SSID", "MyWiFi-Pass", PayloadGenerator.WiFi.Authentication.WPA);
```

要从该有效负载生成QR码，只需调用“ ToString（）”方法并将其传递给QRCoder。

```csharp
//[...]
QRCodeData qrCodeData = qrGenerator.CreateQrCode(wifiPayload.ToString(), QRCodeGenerator.ECCLevel.Q);
//[...]
```

您也可以使用接受有效载荷作为参数的重载方法。有效负载生成器可以设置QR Code Version（QR码版本）（默认为自动设置），ECC Level（ECC级别）（默认为M）和ECI模式（默认为自动检测）。

```csharp
//[...]
QRCodeData qrCodeData = qrGenerator.CreateQrCode(wifiPayload);
//[...]
```

或者，如果要覆盖由有效负载生成器设置的ECC级别，则可以使用重载方法，该方法允许设置ECC级别。

```csharp
//[...]
QRCodeData qrCodeData = qrGenerator.CreateQrCode(wifiPayload, QRCodeGenerator.ECCLevel.Q);
//[...]
```


您可以[在我们的Wiki中](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators)了解有关有效负载生成器的更多[信息](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators)。

PayloadGenerator支持以下类型的有效负载：

* [BezahlCode](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#31-bezahlcode)
* [Bitcoin Payment Address](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#32-bitcoin-payment-address)
* [Bookmark](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#33-bookmark)
* [Calendar events (iCal/vEvent)](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#34-calendar-events-icalvevent)
* [ContactData (MeCard/vCard)](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#35-contactdata-mecardvcard)
* [Geolocation](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#36-geolocation)
* [Girocode](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#37-girocode)
* [Mail](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#38-mail)
* [MMS](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#39-mms)
* [Monero address/payment](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#310-monero-addresspayment)
* [One-Time-Password](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#311-one-time-password)
* [Phonenumber](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#312-phonenumber)
* [Shadowsocks configuration](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#313-shadowsocks-configuration)
* [Skype call](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#314-skype-call)
* [SlovenianUpnQr](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#315-slovenianupnqr)
* [SMS](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#316-sms)
* [SwissQrCode (ISO-20022)](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#317-swissqrcode-iso-20022)
* [URL](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#318-url)
* [WhatsAppMessage](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#319-whatsappmessage)
* [WiFi](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators#320-wifi)
