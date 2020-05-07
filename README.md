# cordova-plugin-prerequisites-checker

Cordova plugin that checks the required prerequisites in order to run corodova app in the hosting cotainer (iOS, Android).

The idea for the project started due to a problematic and non-expected behavoir of Cordova app in different configurations of the same OS.

the plugin target is to verify that all system dependencies are at place.
of not the plugin will direct the user for installing or upgrading the missing components / functionalities.

In most cases when dependencies as such are missing the user will probably will be displyed with the white screen of death for your Cordova applications. 

a great thanks to the contribution of ideas to [cordova-plugin-webview-checker](https://github.com/NoNameProvided/cordova-plugin-webview-checker)

## Installation

```
cordova plugin add https://github.com/oferDavidyan/cordova-plugin-prerequisites-checker.git
```

## Usage

**Add the following snippet into your index.html**
```
<script lang="javascript">
    var requisites = 
            {
                androidApiVersion : "24" ,
                iosApiVersion : " ",
                applicationStoreRedirectMessage : "Ooopps, its seems something is missing, please install from application market"
            };
    
    function onDevReady(){
        plugins.prerequisitesChecker.isConformingToPreRequisites(requisites);
    }
    
    document.addEventListener('deviceready', onDevReady, false);
</script>
```


**tl,dr;**

- use ES5 code only
- put your code for this plugin directly 
  - into the `index.html`
  - into a separate js file what is included before the application code in `index.html`  
- avoid the list of known issues in the quirks section
- you need to wait for the `deviceready` signal to start using the API

This plugin is used to detect the version of the WebView before launching your app, it should not be used in your application code, but directly added to `index.html` via a script tag (inline or not) to render an error state if the required Webview version is not present.

**Usage of language features above ES5**

Do not use anything else than ES5 to write your handler logic for this plugin and do not bundle it into your main application. The reason for this is that the older versions of webview do not support most of the ES6+ features and can only interpret ES5 code.

### Known issues and quirks

Do not pass the any `console` function as a parameter in a promise handler (like `.then(console.log)`). This will cause an `Uncaught TypeError: Illegal invocation` error in older versions of the Android System WebView.

## Notes

The Android System WebView was first shipped in Android 4.4 (KitKat) and based on Chrome for Android 30. Every phone which comes with Google Play installed has it From Android 4.4. In Android 5.0 (Lollipop) the WebView has been decoupled from the OS and become updatable through the Play Store. Above Android 7.0 the Android System WebView will be disabled and won't be updated as long as Chrome is installed and enabled.

### Pre-installed Versions


Every Android release since Android 4.4 (KitKat) comes pre-installed with Android System WebView. The pre-installed versions are the followings:

- Android 4.4 (KitKat) - Android System WebView `30.0.0.0`
- Android 4.4.3 - Android System WebView `33.0.0.0`
- Android 5.0 (Lollipop) - Android System WebView `37.0.0.0`
- Android 6.0 (Marshmallow) - Android System WebView `44.0.2403.117`
- Android 7.0 (Nougat) - disabled by default as Chrome 51+ is used for rendering
- Android 8.0 (Oreo) - disabled by default as Chrome 51+ is used for rendering
- Android 9.0 (Q) - disabled by default as Chrome 51+ is used for rendering


## API


### setPreRequisitesAndroid(androidApiVersion?: string))

Returns a promise which will be resolved to `true` if the Android System Webview is enabled or `false` otherwise.

```js
plugins.preRequisitesChecker.setPreRequisitesAndroid("24");
```

--- 


### setPreRequisitesIOS(iOSApiVersion?: string)

Returns a promise which will be resolved to `true` if the Android System Webview is enabled or `false` otherwise.

```js
plugins.preRequisitesChecker.setPreRequisitesiOS("13.4.1");
```
--- 




### isConformingToPreRequisites()

Returns a promise which will be resolved to `true` if the Android System Webview is enabled or `false` otherwise.

```js
plugins.preRequisitesChecker.isConformingToPreRequisites()
  .then(function(enabled) { console.log(enabled); })
  .catch(function(error) { console.error(error); });
```

> **Note:** Technically all Android phone has the Android System WebView installed but when it's disabled it will default to the version shipped with the phone. So you should determine a minimum required version and use `plugins.preRequisitesChecker.getCurrentWebViewPackageInfo()` function to check if the installed version is higher or not.

--- 


### openApplicationMarketPage(packageName?: string)

A helper function to open the Google Play page or Apple App Store the specified package name / application name.

```js
plugins.preRequisitesChecker.openApplicationMarketPage()
  .then(function() { console.log('Google Play page has been opened.'); })
  .catch(function(error) { console.error(error); });
```

## License

[MIT](./LICENSE)

[npm-package-url]: https://www.npmjs.com/package/cordova-plugin-prerequisites-checker
