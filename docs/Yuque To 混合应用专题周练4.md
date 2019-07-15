### 一、请选择一个你比较熟悉的 cordova 插件，介绍其功能、使用方式及运行原理。

> 参考文章：[《Cordova 官方文档 -- 添加插件》](https://cordova.apache.org/docs/en/latest/guide/cli/index.html#add-plugins)

我们可以在 [插件搜索页](https://cordova.apache.org/plugins/) 中搜索自己需要的插件，也可以通过 CLI 来启动搜索页面：

```shell
$ cordova plugin search statusbar
```

接下来以 `statusbar` 插件为例：   

> 参考文章：[《cordova-plugin-statusbar》](https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-statusbar/index.html)

#### 1.1 插件介绍和安装

`StatusBar` 插件提供了一些自定义 iOS 和 Android `StatusBar` 的功能。

虽然该对象注入到全局作用域 `navigator`，但在 `deviceready` 事件发生后才可用。

```js
document.addEventListener("deviceready", onDeviceReady, false);
function onDeviceReady() {
    console.log(StatusBar);
}
```

安装插件（注意：需要 **cordova 5.0+**）:   

```shell
$ cordova plugin add cordova-plugin-statusbar
```

#### 1.2 插件使用( iOS )

* `StatusBarOverlaysWebView`（布尔值，默认为 `true`）。

在 iOS 上，使状态栏覆盖或不在启动时覆盖 WebView。

```html
<preference name="StatusBarOverlaysWebView" value="true" />
```

* `StatusBarBackgroundColor`（颜色十六进制字符串，无默认值）。

在 iOS 上，在启动时通过十六进制字符串（`#RRGGBB`）设置状态栏的背景颜色。如果未设置此值，则背景颜色将是透明的。

```html
<preference name="StatusBarBackgroundColor" value="#000000" />
```

* `StatusBarStyle`（状态栏样式，默认为 `lightcontent`）。

在 iOS 上，设置状态栏样式。可用选项默认，`lightcontent`，`blacktranslucent`，`blackopaque`。

```html
<preference name="StatusBarStyle" value="lightcontent" />
```

* `StatusBarDefaultScrollToTop`（布尔值，默认为 `false`）。

在 iOS 上，允许Cordova WebView使用默认的滚动到顶部行为。默认为`false`，因此您可以收听“ `statusTap `”事件（如下所述）并自定义行为。

```html
<preference name="StatusBarDefaultScrollToTop" value="false" />
```


#### 1.3 插件使用( web )

在 Android5+ 以后可以指定使用与主应用颜色不同的状态栏颜色：

```js
if (cordova.platformId == 'android') {
    StatusBar.backgroundColorByHexString("#333");
}
```

也可以设置状态栏为半透明，Android 使用十六进制 **ARGB值**，格式为 `#AARRGGBB`。   

例如，黑色状态栏的不透明度为20％：

```js
if (cordova.platformId == 'android') {
    StatusBar.overlaysWebView(true);
    StatusBar.backgroundColorByHexString('#33000000');
}
```

#### 1.4 StatusBar 提供 API

**方法 API：**

* `StatusBar.overlaysWebView`

作用：使状态栏覆盖或不覆盖 WebView；
描述：设置为 `true` 可将状态栏覆盖在应用顶部，反之不会；
支持：`iOS 7+` / `Android 5+`；
示例：   
```js
StatusBar.overlaysWebView(true);
```

* `StatusBar.styleDefault`

作用：使用默认状态栏（深色文本，浅色背景）；
支持：`iOS` / `Android 6+`；
示例：   
```js
StatusBar.styleDefault();
```

* `StatusBar.styleLightContent`

作用：使用 `lightContent` 状态栏（浅色文本，用于深色背景）；
支持：`iOS` / `Android 6+`；
示例：   
```js
StatusBar.styleLightContent();
```

* `StatusBar.styleBlackTranslucent`

作用：使用 `blackTranslucent` 状态栏（浅色文本，适用于深色背景）；
支持：`iOS` / `Android 6+`；
示例：   
```js
StatusBar.styleBlackTranslucent();
```

* `StatusBar.styleBlackOpaque`

作用：使用 `blackOpaque` 状态栏（浅色文本，用于深色背景）；
支持：`iOS` / `Android 6+`；
示例：   
```js
StatusBar.styleBlackOpaque();
```

* `StatusBar.backgroundColorByName`

作用：在 iOS 上，当您将 `StatusBar.overlaysWebView` 设置为 `false` 时，可以按颜色名称设置状态栏的背景颜色；
描述：设置为 `true` 可将状态栏覆盖在应用顶部，反之不会；
支持：`iOS` / `Android 5+`；
支持颜色：black, darkGray, lightGray, white, gray, red, green, blue, cyan, yellow, magenta, orange, purple, brown。   
示例：   
```js
StatusBar.backgroundColorByName("red");
```

* `StatusBar.backgroundColorByHexString`

作用：通过十六进制字符串设置状态栏的背景颜色；
支持：`iOS` / `Android 5+`；
示例：   
```js
StatusBar.backgroundColorByHexString("#C0C0C0");
```

* `StatusBar.hide`

作用：隐藏状态栏；
支持：`iOS` / `Android`；
示例：   
```js
StatusBar.hide();
```

* `StatusBar.show`

作用：显示状态栏；
支持：`iOS` / `Android`；
示例：   
```js
StatusBar.show();
```

**属性 API：**
* `StatusBar.isVisible`

作用：查看状态栏是否可见；
支持：`iOS` / `Android`；
示例：   
```js
if (StatusBar.isVisible) {
    // do something
}
```

**事件 API：**
* `statusTap`

作用：监听是否已点击状态栏；
支持：`iOS` / `Android`；
示例：   
```js
window.addEventListener('statusTap', function() {
    // scroll-up with document.body.scrollTop = 0; or do whatever you want
});
```


### 二、请介绍一下开发 cordova 插件的主要流程和注意事项。

> 参考文章：[《cordova插件的创建》](https://blog.csdn.net/wx_lanyu/article/details/84782646)

#### 2.1 安装插件管理脚手架

```shell
$ npm install -g plugman
```

#### 2.2 创建一个插件

```shell
$ plugman create --name <pluginName> --plugin_id <pluginID> --plugin_version <version> [--path <directory>] [--variableNAME=VALUE]
```
**参数:**
* `pluginName`: 插件名字；
* `pluginID`: 插件id；
* `version`: 版本；
* `directory`:一个绝对或相对路径的目录，该目录将创建插件项目；
* `variable NAME=VALUE`: 额外的描述，如作者信息和相关描述；

例如：   

```shell
$ plugman create --name Leo --plugin_id com.leo.getleo --plugin_version 1.0.1
```
在 src 文件夹下创建 Android 文件夹(因为写的是 Android 的插件)

里面的 libs 和 java 文件是之前项目中拷贝过来的（已经编译通过，并测试没有问题的）

目录结构：

![20190714Hybrid1](http://images.pingan8787.com/20190714Hybrid1.png)

* src 用于存放各个平台的本地代码，我的只有 Android 平台的，所以只有一个 Android 文件夹。

* www 用于存放js文件，js文件主要做接口用（一般使用命令创建的时候会自动生成一个，但是需要修改）。

* `plugin.xml` 文件是插件的配置文件，至关重要。

我们将编写好的 `.java` 文件放入到 src 中的 Android 文件夹中。

修改 www 文件夹下的` .js` 文件, 这里具体的参数, 复制网上搜的, 写的比较详细。

```js
/**
* cordova.define 的第一个参数就是cordova_plugins.js里面定义的id
* exec方法参数说明：
* 参数1：成功回调function
* 参数2：失败回调function
* 参数3：feature name，与config.xml中注册的一致
* 参数4：调用java类时的action
* 参数5：要传递的参数，json数组格式
* 下面提供三种实现方式，三种实现方式均可行
*/
cordova.define("fxp-cordova-plugins.ToastDemo",
    function(require, exports, module) {
 
        /*
         * 实现方式一
        */
        var exec = require("cordova/exec");
        module.exports = {
            toast: function(content){
                exec(null,null,"ToastDemo","toast",[content]);
            },
            toastWithCallback: function (content, successCallback, errorCallback) {
                cordova.exec(successCallback, errorCallback, "ToastDemo", "toastWithCallback", [content]);
            }
        }
 
        /*
         * 实现方式二
        */
/*        var exec = require('cordova/exec');
        var FXP = function(){};
        FXP.prototype.toast=function(content) {
            exec(null, null, "ToastDemo", "toast", [content]);
        };
        FXP.prototype.toastWithCallback=function(content,success, error) {
            exec(success, error, "ToastDemo", "toastWithCallback", [content]);
        };
        var fxp = new FXP();
        module.exports = fxp;*/
 
        /*
         * 实现方式三
        */
/*        var exec = require('cordova/exec');
        exports.toast = function(content) {
            exec(null, null, "ToastDemo", "toast", [content]);
        };
        exports.toastWithCallback=function(content, successCallback, errorCallback){
            exec(successCallback,errorCallback,"ToastDemo","toastWithCallback",[content]);
        };*/
});
```
然后修改 `plugin.xml` 文件：   

```xml
 <!--添加我们自己的平台  -->
<platform name="android">  <!-- 平台类型 -->
    <config-file target="res/xml/config.xml" parent="/*">  
        <feature name="Leo">  <!-- JS调用时的前缀名字 -->
            <param name="android-package" value="com.leo.maploction.Leo"/> <!-- .java类名全路径 --> 
        </feature>  
    </config-file>  
    
    <config-file target="AndroidManifest.xml" parent="/*">
                            <!-- android所需权限 -->
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    </config-file>
    
    <source-file src="src/android/Leo.java" target-dir=""src/com/Leo/getleo"/>
    
    <!-- src:java源文件的路径， target-dir:插件安装好后，源文件的位置，要和上面的包名对应 -->	
</platform> 
```

然后输入 `npm init` 来创建 `package.json` 文件，信息可以自定义,也可以直接回车使用默认的：   

![20190714Hybrid2](http://images.pingan8787.com/20190714Hybrid2.jpg)

### 三、请选择一个你比较熟悉的 ionic-native 插件，介绍其功能、使用方式及运行原理。

Ionic Native 对开源的 Cordova 插件进行了封装，让我们更容易的在 Ionic 中使用 Cordova 插件。

Ionic Native 将 Cordova 封装成 Promise or Observable 的对象，并为所有的插件提供一个通用的接口。

让我们更方便的在 Ionic 调用手机原生的功能。 Cordova 能实现的功能在 Ionic 中都可以实现。

接下来以 `StatusBar` 插件为例：

> 参考文章：[《Status Bar》](http://www.ionic.wang/native_doc-index-id-263.html)

#### 3.1 插件介绍和安装

该 `StatusBar` 对象提供了一些自定义 iOS 和 Android  `StatusBar` 的功能。

这里可以查看 Cordova 的 StatusBar 文档：[查看 StatusBar](https://github.com/apache/cordova-plugin-statusbar)。

#### 3.2 插件安装

```shell
$ ionic cordova plugin add cordova-plugin-statusbar
$ npm install @ionic-native/status-bar
```

查询当前项目有哪些插件：

```shell
ionic plugin list
```

插件支持平台： 


#### 3.3 插件使用

添加插件：
```js
// app.module.ts文件
import { NgModule, ErrorHandler } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { IonicApp, IonicModule, IonicErrorHandler } from 'ionic-angular';
import { MyApp } from './app.component';


import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';
import {Camera} from "@ionic-native/camera";

@NgModule({
  declarations: [
    MyApp,
  ],
  imports: [
    BrowserModule,
    IonicModule.forRoot(MyApp)
  ],
  bootstrap: [IonicApp],
  entryComponents: [
    MyApp,
  ],
  providers: [
    StatusBar,
    // 添加插件
    Camera,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler}
  ]
})
export class AppModule {}
```

使用插件
```js
// home.ts使用文件
import { StatusBar } from '@ionic-native/status-bar/ngx';

export class HomePage {
    constructor(private statusBar: StatusBar) { }

    // let status bar overlay webview
    this.statusBar.overlaysWebView(true);

    // set status bar to white
    this.statusBar.backgroundColorByHexString('#ffffff');
}
```

### 四、请介绍一下开发 ionic-native 插件的主要流程和注意事项。


### 五、请分析一下 ionic-native 插件和 cordova 插件是如何协作的。