### 一、在 Android 平台下如何调试 WebView？

#### 1. 在 Chrome 浏览器上调试
> 参考文章：[《Android调试webview》](https://www.jianshu.com/p/3591eebbe797)     


**1.1 条件：**   
* 在 Android 设备或模拟器运行 Android4.4 或更高版本，Android 设备上启用 **USB调试模式**。

* Chrome 30 或更高版本。更强大的 WebView 界面调试功能需要 Chrome31 或更高版本。

* Android 应用程序中的 WebView 配置为**可调试模式**。

**1.2 Android 代码中配置 WebView 为可调试：**

```java
if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {  
    WebView.setWebContentsDebuggingEnabled(true);  
}  
```

注意 web 调测不受 `app manifest` 文件中 `debuggable` 标记状态的影响，如果希望仅 `debuggable` 为 `true` 时才能使用 web 调测，那么运行时检测此标记，如下：

```java
if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {  
    if ( 0 != ( getApplcationInfo().flags &= ApplicationInfo.FLAG_DEBUGGABLE ) ) {  
        WebView.setWebContentsDebuggingEnabled(true);  
    }  
}  
```

**1.3 手机开启 USB 调试选项，并用 USB 连接电脑：**   

开启Android手机的开发者选项，一般在系统设置 - Android版本进行多次点击会触发开启开发者选项，然后进入开发者选项页面，开启USB调试。

为了避免每次调试时看到此警告，勾选“总是允许从这台计算机”，并单击“确定”。

**1.4 在 Chrome 中启用设置“USB web debugging”（不会影响WebView）：**  


在 Chrome 上访问 `chrome://inspect/#devices` 或 `about:inspect` 访问已启用调试的 WebView 列表，需要翻墙。

然后在 WebView 列表中选择你要调试的页面，点击“ `Inspect` ”选项，跟调试 PC 网页一样，使用 Chrome 控制台进行调试。   

![Chrome 控制台](http://images.pingan8787.com/20190707hybrid01.png)

![Chrome 控制台](http://images.pingan8787.com/20190707hybrid02.png)

**1.5 小技巧：**   

（1）访问 `chrome://inspect/#devices` 如果 chrome 没有检测到 `Remote Target` 中的页面，可能需要安装一下 Chrome 的 ADB 插件，也可以在 Chrome 翻墙访问 `https://chrome-devtools-frontend.appspot.com`；

（2）对于腾讯系的 APP，默认采用 X5内核 ，需要将 `WebViewDebugHook` 的 git 目录下的 `debug.conf` 文件拷贝到 SD卡 的根目录下即可。


#### 2. 使用 DebugGap 调试

> 参考文章：[《Android下的webview调试》](https://segmentfault.com/a/1190000009240637)

**2.1 Windows 下载 DebugGap 并配置：**   

在电脑上面下载 Windows 版本的 `DebugGap` 软件包（下载链接：[DebugGap](http://www.debuggap.com/)），下载完成后解压下来。

安装完成后，运行 `DebugGap` ，开始配置：

* 通常情况下，`DebugGap` 可以自动获取IP，并设置默认的端口，如果没有，你可以手动设置；
* 点击“**连接**”按钮启动各种客户端的侦听器；

![DebugGap](http://images.pingan8787.com/20190707hybrid03.png)

**2.2 在客户端上配置：**

在调试项目中要进行测试的 HTML 界面中引入 `debuggap.js`。
```html
<script src="debuggap.js" type="text/javascript"></script>
```

当调试项目的加载时，您的应用程序将会有一个蓝色的地方，点击会出现一个四叶三叶草的东西，点击“**配置**”，显示配置页面。输入与远程 DebugGap 上的主机和端口相同的主机和端口，例如 `192.168.1.4:11111`，然后点击“**连接**”按钮。

![DebugGap](http://images.pingan8787.com/20190707hybrid04.png)

1.4电脑端远程 DebugGap 将检测即将到来的客户端，开发人员可以单击每个客户端进行调试。

![DebugGap](http://images.pingan8787.com/20190707hybrid05.png)

### 二、在 iOS 平台下如何调试 WebView？

> 参考文章：[《iOS之Safari调试webView/H5页面》](https://www.cnblogs.com/dianming/p/6902442.html)

一般我们通过 Mac 的 Safari浏览器 来调试，但是要注意两点：

* 如果调试的是 APP 中 WebView 的页面，则需要这个 APP 的包支持调试，如果不能调试，需要让 iOS 开发人员重签名 APP（可能需要将我们 iOS 设备的 ID 写入到可信任设备列表中，然后使用 iTunes 安装客户端提供的测试包即可）。

* 如果调试的是 H5 页面，可以直接在手机的 Safari浏览器 打开直接调试。   

下面开始说说在 Mac 上如何调试：   

#### 1. 开启 Safari 开发菜单

先将 iPhone 连接到 Mac，在 Mac 的 Safari 偏好设置中，开启开发菜单。具体步骤为：Safari -> 偏好设置… -> 高级 -> 勾选在菜单栏显示“**开发**”菜单。

![Safari](http://images.pingan8787.com/20190707hybrid06.jpg)


#### 2. iPhone 开启 Web检查器

具体步骤为：设置 -> Safari -> 高级 -> Web 检查器。

![Safari](http://images.pingan8787.com/20190707hybrid07.jpg)

#### 3. 调试 APP 内的 WebView

> 参考文章：[《前端 WEBVIEW 指南之 IOS 调试篇》](https://imnerd.org/ios-webview-debug.html)

在 Safari-> 开发中，看到自己的设备以及 WebView 中网页，点击后即可开启对应页面的 `Inspector` ，可以用来进行断点调试。   

![Safari](http://images.pingan8787.com/20190707hybrid09.png)

![Safari](http://images.pingan8787.com/20190707hybrid10.png)



### 三、在内嵌版调试过程中，Fiddler 或 Charles 能起到什么作用？ 

这两者都是强大的抓包工具，原理是以**web代理服务器**的形式进行工作的，使用的代理地址是：`127.0.0.1`，端口默认为`8888`，我们也可以通过设置进行修改。

**代理**就是在客户端和服务器之间设置一道关卡，客户端先将请求数据发送出去后，代理服务器会将数据包进行拦截，代理服务器再冒充客户端发送数据到服务器；同理，服务器将响应数据返回，代理服务器也会将数据拦截，再返回给客户端。

Fiddler 或 Charles 的主要作用有：   

* 可以代理请求，用来查看页面发送的请求和接收的响应；
* 可以修改请求的响应，用来模拟自己想要的数据；
* 可以模拟网络请求的速度；
* 可以代理服务器的静态资源请求，指向本地文件，省去频繁发布 H5 包，达到快速调试目的；

> 补充链接：[《Fiddler工具使用介绍一》](https://www.cnblogs.com/miantest/p/7289694.html)

### 四、调试企业微信、微信和钉钉版时，可以使用哪些工具？

#### 1. 调试企业微信、微信

* **[微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)**，可以用来调试页面基本功能；
* **[企业微信接口调试工具](https://work.weixin.qq.com/api/devtools/devtool.php)**，可以用来调试企业微信的接口；

#### 2. 调试钉钉

* **[钉钉Android开发版](https://files.alicdn.com/tpsservice/76ac853fc96075414a837d8bbae89030.pdf)**，用来调试Android上的钉钉应用；

#### 3. 通用

* **Fiddler 或 Charles**，可以拦截接口替换文件，来调试应用；

### 五、常见的调试技巧有哪些？

#### 1. Chrome 控制台调试

> 参考文章：[《前端常见调试技巧篇总结（持续更新...）》](https://www.cnblogs.com/chenbeibei520/p/9959555.html)

**1.1 Source 面板断点调试 JS**  
从左到右，各个图标表示的功能分别为：

* `Pause/Resume script execution`：暂停/恢复脚本执行（程序执行到下一断点停止）。
* `Step over next function call`：执行到下一步的函数调用（跳到下一行）。
* `Step into next function call`：进入当前函数。
* `Step out of current function`：跳出当前执行函数。
* `Deactive/Active all breakpoints`：关闭/开启所有断点（不会取消）。
* `Pause on exceptions`：异常情况自动断点设置。

**1.2 Element 面板调试 DOM：**   

右击元素，选择 `break on` 选项：  

![break on](http://images.pingan8787.com/20190707hybrid14.png)

* `Subtree modifications` 选项，是指**当节点内部子节点变化时断点**；    

![break on](http://images.pingan8787.com/20190707hybrid11.gif)

* `Attributes modifications` 选项，是指**当节点属性发生变化时断点**；   

![break on](http://images.pingan8787.com/20190707hybrid12.gif)

* `node removal` 选项，是指**当节点被移除时断点**；   

![break on](http://images.pingan8787.com/20190707hybrid13.png)


#### 2. console 调试

> 参考文章：[《Console调试常用用法》](https://blog.csdn.net/a0405221/article/details/85248433)

**2.1 显示信息的命令：** 

```js
console.log("normal");         // 用于输出普通信息
console.info("information");   // 用于输出提示性信息
console.error("error");        // 用于输出错误信息
console.warn("warn");          // 用于输出警示信息
console.clear();               // 清空控制台信息
```

**2.2 计时功能：**   

`console.time()` 和 `console.timeEnd()` ：

```js
console.time("控制台计时器");
for(var i = 0; i < 10000; i++){
    for(var j = 0; j < 10000; j++){}       
}
console.timeEnd("控制台计时器")
```

**2.3 信息分组：**   

`console.group()` 和 `console.groupEnd()`：   

```js
console.group("第一组信息");
    console.log("第一组第一条：我的博客");
    console.log("第一组第二条：CSDN");
console.groupEnd();

console.group("第二组信息");
    console.log("第二组第一条：程序爱好者QQ群");
    console.log("第二组第二条：欢迎你加入");
console.groupEnd();
```

**2.4 将对象以树状结构展现：**   

`console.dir()` 可以显示一个对象所有的属性和方法：   

```js
var info = {
    name : "Alan",
    age : "27",
    grilFriend : "nothing",
    getName : function(){
        return this.name;
    }
}
console.dir(info);
```

**2.5 显示某个节点的内容：**   

`console.dirxml()` 用来显示网页的某个节点( `node`) 所包含的 `html/xml` 代码：   

```js
var node = document.getElementById("info");
node.innerHTML += "<p>追加的元素显示吗</p>";
console.dirxml(node);
```

**2.5 统计代码被执行的次数：**  

使用 `console.count()`：   

```js
function myFunction(){
    console.count("myFunction 被执行的次数");
}
myFunction();       //myFunction 被执行的次数: 1
myFunction();       //myFunction 被执行的次数: 2
myFunction();       //myFunction 被执行的次数: 3
```

**2.6 输出表格：**   

```js
console.table(mytable);
```