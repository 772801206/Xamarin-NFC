# ![NFC logo][logo] Plugin.NFC
A Cross-Platform NFC (Near Field Communication) plugin to easily read and write NFC tags in your application.

## Build Status
Type|Status|NuGet|MyGet
:---|:---|:---|:---
Plugin.NFC | [NC] | [NC] | [NC]

## Supported Platforms
Platform|Version|Development Status|Tested on
:---|:---|:---|:---
Android|4.4+|Working|Google Nexus 5, Huawei Mate 10 Pro
iOS|11+|_**Work In Progress**_|No iOS devices available
Windows|10.0.16299+|_**Work In Progress**_|No Windows devices available

## Setup
### Android Specific
* Add NFC Permission `android.permission.NFC` and NFC feature `android.hardware.nfc` in your `AndroidManifest.xml`
```xml
<uses-permission android:name="android.permission.NFC" />
<uses-feature android:name="android.hardware.nfc" android:required="false" />
```

* Add the line `CrossNFC.Init(this)` in your `OnCreate()`
```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    TabLayoutResource = Resource.Layout.Tabbar;
    ToolbarResource = Resource.Layout.Toolbar;
    base.OnCreate(savedInstanceState);
    
    // Plugin NFC: Initialization
    CrossNFC.Init(this);

    global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
    LoadApplication(new App());
}
```

* Add the line `CrossNFC.OnNewIntent(intent)` in your `OnNewIntent()`
```csharp
protected override void OnNewIntent(Intent intent)
{
    base.OnNewIntent(intent);

    // Plugin NFC: Tag Discovery Interception
    CrossNFC.OnNewIntent(intent);
}
```

### iOS Specific

> WARNING : DUE TO APPLE RESTRICTION, ONLY READING TAG IS AVAILABLE.

An iPhone 7+ and iOS 11+ are required in order to use NFC with iOS devices.
* Add `Near Field Communication Tag Reading` capabilty in your `Entitlements.plist`
```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
    <string>NDEF</string>
</array>
```

### Windows Specific

> WARNING : ONLY NDEF IS SUPPORTED.

* Add `Proximity` capability to the `Package.appxmanifest`
```xml
<Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="proximity" />
</Capabilities>
```

## API Usage

Before to use the plugin, please check if NFC feature is supported by the platform using `CrossNFC.IsSupported`.

To get the current platform implementation of the plugin, please call `CrossNFC.Current`:
* Check `CrossNFC.Current.IsAvailable` to verify if NFC is available.
* Check `CrossNFC.Current.IsEnabled` to verify if NFC is enabled. 
* Register events:
```csharp
// Event raised when a ndef message is received.
CrossNFC.Current.OnMessageReceived += Current_OnMessageReceived;
// Event raised when a ndef message has been published.
CrossNFC.Current.OnMessagePublished += Current_OnMessagePublished;
// Event raised when a tag is discovered. Used for publishing.
CrossNFC.Current.OnTagDiscovered += Current_OnTagDiscovered;
```

### Read a tag
* Start listening with `CrossNFC.Current.StartListening()`.
* When a NDEF message is received, the event `OnMessageReceived` is raised.

### Write a tag
* To write a tag, call `CrossNFC.Current.StartPublishing()`, then `CrossNFC.Current.PublishMessage(ITagInfo)` when tag is discovered. Do not forget to call `CrossNFC.Current.StopPublishing()` once the tag has been written.

### Clear a tag
* To clear a tag, call `CrossNFC.Current.StartPublishing(clearMessage: true)`, then `CrossNFC.Current.PublishMessage(ITagInfo)` when tag is discovered. Do not forget to call `CrossNFC.Current.StopPublishing()` once the tag has been cleared.


For more examples, see sample application in the repository.

## Credits
Special thanks to:
- James Montemagno : [GitHub](https://github.com/jamesmontemagno)
- Matthew Leibowitz ([Xamarin.Essentials PR #131](https://github.com/xamarin/Essentials/pull/131)) : [GitHub](https://github.com/mattleibow)
- Alessandro Pozone ([NFCForms](https://github.com/poz1/NFCForms)) : [GitHub](https://github.com/poz1)
- Ultz ([XNFC](https://github.com/Ultz/XNFC)) : [GitHub](https://github.com/Ultz)
- Sven-Michael St�be ([xamarin-nfc](https://github.com/smstuebe/xamarin-nfc)) : [GitHub](https://github.com/smstuebe)

## License
```
The MIT License (MIT)
 
Copyright (c) 2019 Franck Bour

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```


[logo]: https://github.com/franckbour/Plugin.NFC/raw/master/art/nfc48.png "NFC Logo"