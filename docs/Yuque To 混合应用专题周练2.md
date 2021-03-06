### 一、iOS 平台中 UIWebView 与 WKWebView 有什么区别？

> 参考文章：[《UIWebView与WKWebView》](http://www.cocoachina.com/articles/17337)

`UIWebView` 是苹果继承于UIView封装的一个加载 web 内容的类,它可以加载任何远端的web数据展示在你的页面上，你可以像浏览器一样前进后退刷新等操作。不过苹果在 iOS8 以后推出了 `WKWebView` 来加载 Web，并应用于 iOS 和 OSX 中，它取代了 `UIWebView` 和 `WebView` ，在两个平台上支持同一套 API。

它脱离于 `UIWebView` 的设计，将原本的设计拆分成14个类，和3个代理协议，虽然是这样但是了解之后其实用法比较简单，依照职责单一的原则，每个协议做的事情根据功能分类。

![UIWebView](http://cdn.cocimg.com/ueditor/upload/image/20160810/1470807807124957.png)

`WKWebView` 相比于 `UIWebView`：  

* `WKWebView` 的内存远远没有 `UIWebView` 的开销大,而且没有缓存；

* 拥有高达 60FPS 滚动刷新率及内置手势；

* 支持了更多的 HTML5 特性；

* 高效的 app 和 web 信息交换通道；

* 允许 `JavaScript` 的 `Nitro` 库加载并使用, `UIWebView` 中限制了；

* `WKWebView` 目前缺少关于页码相关的 API；

* 提供加载网页进度的属性；

**小结：**   

`WKWebView` 相较于 `UIWebView` 在整体上有较大的提升，满足 iOS 上面使用同一套控件的功能，同时对整个内存的开销以及滚动刷新率和 JS 交互做了优化的处理。

依据职责单一的原则，拆分成了三个协议去实现 `WebView` 的响应，解耦了 JS 交互和加载进度的响应处理。 

`WKWebView` 没有做缓存处理,所以对网页需要缓存的加载性能要求没那么高的还是可以考虑 `UIWebView` 。

### 二、WKWebView 有哪一些坑？

> 参考文章：[《WKWebView 那些坑》](https://kknews.cc/tech/x2rzg3g.html)


#### 1. WKWebView 白屏问题

`WKWebView` 实际上是个多进程组件，这也是它加载速度更快，内存暂用更低的原因。

在 `UIWebView` 上当内存占用太大的时候，App Process 会 crash；而在 `WKWebView` 上当总体的内存占用比较大的时候，WebContent Process 会 crash，从而出现白屏现象。

**解决办法：**

1. **借助 WKNavigtionDelegate**  

当 `WKWebView` 总体内存占用过大，页面即将白屏的时候，系统会调用上面的回调函数，我们在该函数里执行`[webView reload]`(这个时候 `webView.URL` 取值尚不为` nil`）解决白屏问题。在一些高内存消耗的页面可能会频繁刷新当前页面，H5侧也要做相应的适配操作。

2. **检测 webView.title 是否为空**

并不是所有 H5 页面白屏的时候都会调用上面的回调函数，比如，最近遇到在一个高内存消耗的 H5 页面上 present 系统相机，拍照完毕后返回原来页面的时候出现白屏现象（拍照过程消耗了大量内存，导致内存紧张，WebContent Process 被系统挂起），但上面的回调函数并没有被调用。在 `WKWebView` 白屏的时候，另一种现象是 `webView.titile` 会被置空, 因此，可以在 `viewWillAppear` 的时候检测 `webView.title` 是否为空来 `reload` 页面。


#### 2. WKWebView Cookie 问题

`WKWebView` `Cookie` 问题在于 `WKWebView` 发起的请求不会自动带上存储于 `NSHTTPCookieStorage` 容器中的 `Cookie`，而在 `UIWebView` 会自动带上 `Cookie`。  

#### 3. WKWebView loadRequest 问题

在 `WKWebView` 上通过 `loadRequest` 发起的 `post` 请求 `body` 数据会丢失：

#### 4. WKWebView NSURLProtocol问题

`WKWebView` 在独立于 app 进程之外的进程中执行网络请求，请求数据不经过主进程，因此，在 `WKWebView` 上直接使用 `NSURLProtocol` 无法拦截请求。

#### 5. WKWebView 页面样式问题

在 `WKWebView` 适配过程中，我们发现部分 H5 页面元素位置向下偏移或被拉伸变形，追踪后发现主要是 H5 页面高度值异常导致。

#### 6. WKWebView 截屏问题

WKWebView 下通过 -[CALayer renderInContext:]实现截屏的方式失效，需要通过以下方式实现截屏功能：

```
@implementation UIView (ImageSnapshot) 
- (UIImage*)imageSnapshot { 
    UIGraphicsBeginImageContextWithOptions(self.bounds.size,YES,self.contentScaleFactor);
    [self drawViewHierarchyInRect:self.bounds afterScreenUpdates:YES]; 
    UIImage* newImage = UIGraphicsGetImageFromCurrentImageContext(); 
    UIGraphicsEndImageContext(); 
    return newImage; 
} 
@end
```

然而这种方式依然解决不了 `webGL` 页面的截屏问题，截屏结果不是空白就是纯黑图片。  

无奈之下，我们只能约定一个JS接口，让游戏开发商实现该接口，具体是通过 `canvas` `getImageData()`方法取得图片数据后返回 `base64` 格式的数据，客户端在需要截图的时候，调用这个JS接口获取` base64 String `并转换成 `UIImage`。

#### 7. WKWebView crash问题

如果 `WKWebView` 退出的时候，JS刚好执行了 `window.alert()`,  alert 框可能弹不出来，`completionHandler` 最后没有被执行，导致` crash`；

另一种情况是在 `WKWebView` 一打开，JS就执行 `window.alert()`，这个时候由于 `WKWebView` 所在的 `UIViewController` 出现（ `push` 或 `present` ）的动画尚未结束，alert 框可能弹不出来，`completionHandler `最后没有被执行，导致 `crash`。   

#### 8. 视频自动播放

`WKWebView` 需要通过 `WKWebViewConfiguration.mediaPlaybackRequiresUserAction` 设置是否允许自动播放，但一定要在 `WKWebView` 初始化之前设置，在 `WKWebView` 初始化之后设置无效。


#### 9. goBack API问题

`WKWebView` 上调用 `-[WKWebView goBack]`, 回退到上一个页面后不会触发`window.onload()` 函数、不会执行JS。

#### 10. 页面滚动速率

`WKWebView` 需要通过 `scrollView delegate` 调整滚动速率：

```
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView {
    scrollView.decelerationRate = UIScrollViewDecelerationRateNormal; 
}
```

### 三、Crosswalk 是什么，它有什么作用？

> 参考网站： [《Crosswalk Github》](https://Crosswalk-project.org/) 
> 参考文章： [《Crosswalk入门》](https://www.mobibrw.com/2015/1934)

**Crosswalk** 是一款开源的 web 引擎。目前 **Crosswalk** 正式支持的移动操作系统包括 Android 和 Tizen ，在 Android 4.0 及以上的系统中使用 **Crosswalk** 的 Web 应用程序在 HTML5 方面可以有一致的体验，**同时和系统的整合交互方面（比如启动画面、权限管理、应用切换、社交分享等等）可以做到类似原生应用**。

现在 **Crosswalk** 已经成为众多知名 HTML5 平台和应用的推荐引擎，包括 Google Mobile Chrome App 、 Intel XDK 、 Famo.us 和 Construct2 等等，未来的 Cordova 4.0 也计划集成 **Crosswalk** 。



### 四、常见的 WebView 性能优化方案有哪一些？

> 参考文章：[《WebView常用优化方案》](https://www.jianshu.com/p/f64e1b1c90d9)

#### 1. WebView 动态加载

WebView 动态加载。就是不在xml中写WebView，写一个layout，然后把WebView add进去。

```java
WebView mWebView = new WebView(getApplicationgContext());
LinearLayout mll = findViewById(R.id.xxx);
mll.addView(mWebView);

protected void onDestroy() {
    super.onDestroy();
    mWebView.removeAllViews();
    mWebView.destroy()
}
```

这里用的 `getApplicationContext()` 也是防止内存溢出，这种方法有一个问题。如果你需要在 `WebView` 中打开链接或者你打开的页面带有 flash，获得你的 `WebView` 想弹出一个 `dialog` ，都会导致从 `ApplicationContext` 到 `ActivityContext` 的强制类型转换错误，从而导致你应用崩溃。

这是因为在加载 flash 的时候，系统会首先把你的 `WebView` 作为父控件，然后在该控件上绘制 flash ，他想找一个 `Activity` 的 `Context` 来绘制他，但是你传入的是 `ApplicationContext` 。然后就崩溃了。

#### 2. 独立的web进程，与主进程隔开

这个方法被运用于类似qq，微信这样的超级app中，这也是解决任何webview内存问题屡试不爽的方法 对于封装的webactivity，在manifest.xml中设置

```html
<activity 
    android:name=".webview.WebViewActivity" 
    android:launchMode="singleTop" 
    android:process=":remote" 
    android:screenOrientation="unspecified" 
/>
```

然后在关闭 `webactivity` 时销毁进程：  

```java
@Overrideprotected void onDestroy() {                
super.onDestroy(); 
    System.exit(0);
}
```
关闭浏览器后便销毁整个进程，这样一般 95% 的情况下不会造成内存泄漏之类的问题，但这就涉及到 android 进程间通讯，比较不方便处理，优劣参半，也是可选的一个方案。   

#### 3. WebView释放

```java
public void destroy() {
    if (mWebView != null) {
        // 如果先调用destroy()方法，则会命中if (isDestroyed()) return;这一行代码，需要先onDetachedFromWindow()，再
        // destory()
        ViewParent parent = mWebView.getParent();
        if (parent != null) {
            ((ViewGroup) parent).removeView(mWebView);
        }
​
        mWebView.stopLoading();
        // 退出时调用此方法，移除绑定的服务，否则某些特定系统会报错
        mWebView.getSettings().setJavaScriptEnabled(false);
        mWebView.clearHistory();
        mWebView.clearView();
        mWebView.removeAllViews();
​
        try {
            mWebView.destroy();
        } catch (Throwable ex) {
​
        }
    }
}
```

### 五、目前职行力 App 的更新方案是什么，有什么区别？

V2 项目：  

* 前端打包，并自动将打包后的文件拷贝到 SP 项目下
* 在 SP 项目，提交打包后的文件到 git
* 打包平台添加版本

具体还得请教下~~~