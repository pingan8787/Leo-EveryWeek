
## 一、OAuth 概念

> 开放授权（OAuth）是一个开放标准，允许用户让第三方应用访问该用户在某一网站上存储的私密的资源（如照片，视频，联系人列表），而无需将用户名和密码提供给第三方应用。 —— 维基百科

严格来说，OAuth2 不是一个标准协议，而**是一个安全的授权框架**。其详细描述系统中不同角色，用户，服务前端应用（如 API ）以及客户端（如网站或APP）之间如何**实现相互认证**。

当前 OAuth 协议版本是 OAuth2.0，需要注意的是，OAuth2.0 并不向下兼容 OAuth1.0。

在生活中，比较常见的 OAuth2 的使用场景是**授权登录**，并且也广泛应用于 web、桌面应用和移动 APP 的**第三方服务提供授权登录验证机制，以实现不同应用直接数据访问的权限**。

## 二、OAuth2 重点名词介绍

在 OAuth2 标准中定义了以下四种角色：

* 资源拥有者 (**Resource Owner**)：

代表授权客户端访问本身资源信息的用户（User）；

* 客户端 (**Client**)：

代表意图访问受限资源的第三方应用。

* 资源服务器 (**Resource Server**)：

代表托管了受保护的用户账号信息的服务器，它与认证服务器，可以是同一台服务器，也可以是不同的服务器；


* 授权服务器 (**Authorization Server**)：

代表验证用户身份然后为客户端派发资源访问令牌的服务器，即服务提供商专门用来处理认证的服务器；

## 三、OAuth2 运行流程

### 1. 流程分析

![20191028-OAuth2-01.png](http://images.pingan8787.com/blog/20191028-OAuth2-01.png)

（配图来自**阮一峰大佬**）

大致流程概括就是：

* （A）Authrization Request

用户打开客户端以后，客户端要求用户给予授权。

* （B）Authorization Grant（Get）

用户同意给予客户端授权。

* （C）Authorization Grant（Post）

客户端向**授权服务器**发送它自己的客户端**身份标识**和上一步中获得的授权（authorization grant），向认证服务器申请令牌。

* （D）Access Token（Get）

认证服务器对客户端进行认证以后，确认无误，同意发放令牌（access token），授权阶段至此全部结束。

* （E）Access Token（Post && Validate）

客户端使用令牌，向资源服务器申请获取资源。

* （F）Protected Resource（Get）

资源服务器确认令牌无误，同意向客户端开放资源。


理解完上面整个流程以后，我们再看看下面这张图，能更加清晰理解 OAuth2 的整个运行流程：

![20191028-OAuth2-02.png](http://images.pingan8787.com/blog/20191028-OAuth2-02.png)

（配图来自公众号**前端修仙之路**）

从整个流程可以看出，在 B 步骤最为关键，即**需要获取到用户对客户端的授权**（如我们在微信扫码登录时，点击“确定”按钮的步骤）。

有了这个授权以后，客户端才能拿到令牌，进而凭令牌向资源服务器获取资源。

### 2. 案例：微信登录

另外，微信登录的实现流程也类似：

![20191028-OAuth2-03.png](http://images.pingan8787.com/blog/20191028-OAuth2-03.png)

（配图来自**微信官方文档**）

其整体流程为： 

1. 第三方发起微信授权登录请求，微信用户允许授权第三方应用后，微信会**拉起应用或重定向到第三方网站**，并且带上授权临时票据 `code` 参数；

2. 通过 `code` 参数加上 `AppID` 和 `AppSecret` 等，通过 API 换取 `access_token` ；

3. 通过 `access_token` 进行接口调用，**获取用户基本数据资源或帮助用户实现基本操作**。

### 3. OAuth2 优缺点

* 优点：

适合快速开发实施，代码量少，API需要被不同APP使用，且每个APP使用方式也不同的情况。

* 缺点：

学习和理解的成本比较大，并且 OAuth2 不是一个严格的标准协议，在实施过程中更容易出错。

## 三、OAuth2 四种授权模式

通过前面描述，可以知道**OAuth 的核心就是向第三方应用颁发令牌。**

OAuth 2.0 规定了四种获得令牌的流程。你可以选择最适合自己的那一种，向第三方应用颁发令牌。即以下四种授权方式：

* 授权码（authorization-code）
* 隐藏式（implicit）
* 密码式（password）
* 客户端凭证（client credentials）

**注意：**

不管哪一种授权方式，第三方应用申请令牌之前，都必须先到系统备案，说明自己的身份，然后会拿到两个身份识别码：**客户端 ID（client ID）**和**客户端密钥（client secret）**。

这是为了防止令牌被滥用，没有备案过的第三方应用，是不会拿到令牌的。

### 1. 授权码

即**第三方应用先申请一个授权码，然后再用该码获取令牌**。

适用于有后端的 Web 应用，授权码通过前端传送，令牌则是储存在后端，而且所有与资源服务器的通信都在后端完成。这样的前后端分离，可以避免令牌泄漏。

这种方式也是最常用的流程，安全性最高。

整体流程如下：

![20191028-OAuth2-04.png](http://images.pingan8787.com/blog/20191028-OAuth2-04.png)

（配图来自**阮一峰大佬**）

步骤分析：

1. 用户从 A 网站跳转到 B 网站，请求用户确认授权，以获取授权码，其发送链接示意如下：

```sh
https://b.com/oauth/authorize?
       response_type=code&
       client_id=CLIENT_ID&
       redirect_uri=CALLBACK_URL&
       scope=read
```
其中：

`response_type` 参数表示要求返回授权码（code）;

`client_id` 参数让 B 知道是谁在请求;

`redirect_uri` 参数是 B 接受或拒绝请求后的跳转网址;

`scope` 参数表示要求的授权范围（这里是只读）;

2. 在 B 网站中，当用户同意授权 A 网站，则 B 网站会携带授权码，重定向到 `redirect_uri` 参数指定的网址，就像下面这样：

```sh
https://a.com/callback?code=AUTHORIZATION_CODE
```

3. A 网站获取授权码以后，在 A 网站后端中向 B 网站请求令牌：

```sh
https://b.com/oauth/token?
       client_id=CLIENT_ID&
       client_secret=CLIENT_SECRET&
       grant_type=authorization_code&
       code=AUTHORIZATION_CODE&
       redirect_uri=CALLBACK_URL
```

`client_id` 参数和 `client_secret` 参数用来让 B 确认 A 的身份（ `client_secret` 参数是保密的，因此只能在后端发请求）;

`grant_type` 参数的值是 `AUTHORIZATION_CODE` ，表示**采用的授权方式是授权码**;

`code` 参数是上一步拿到的授权码;

`redirect_uri` 参数是令牌颁发后的回调网址;

4. B 网站接受请求并验证身份，身份验证通过后，会发放令牌。向`redirect_uri` 指定的网址，发送包含令牌 `access_token` 字段的JSON数据，流程完毕。

### 2. 隐藏式

### 3. 密码式

### 4. 客户端凭证



## 参考文章

1. 部门大佬的资料
2. [《OAuth 2 深入介绍》](https://www.cnblogs.com/Wddpct/p/8976480.html)
3. [《理解OAuth 2.0》](www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)