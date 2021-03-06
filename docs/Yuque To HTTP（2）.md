### 1. 什么是跨域，如何解决跨域

#### 1.1 什么是跨域

跨域，指的是浏览器不能执行其他网站的脚本。它是由**浏览器的同源策略**造成的，是浏览器对JavaScript施加的安全限制。   

**同源策略**限制了以下行为：   

* `Cookie`、`LocalStorage` 和 `IndexDB` 无法读取   
* `DOM` 和 `JS `对象无法获取    
* `Ajax`请求发送不出去  

**常见跨域场景：**   

所谓的同源是指：**域名**、**协议**、**端口**均为相同。

* `http://www.a.cn/index.html` 调用  ` http://www.a.cn/server.php`  非跨域。   

* `http://www.a.cn/index.html` 调用   `http://www.b.cn/server.php`  跨域，主域不同。   

* `http://abc.a.cn/index.html` 调用   `http://def.b.cn/server.php`  跨域，子域名不同。   

* `http://www.a.cn:8080/index.html` 调用   `http://www.a.cn/server.php`  跨域，端口不同。   

* `https://www.a.cn/index.html` 调用   `http://www.a.cn/server.php`  跨域,协议不同。   

* `localhost` 调用 `127.0.0.1` 跨域。   

#### 1.2 解决跨域

1. `jsonp` 跨域
2. `document.domain` + `iframe` 跨域
3. `window.name` + `iframe` 跨域
4. `location.hash` + `iframe` 跨域
5. `postMessage` 跨域
6. 跨域资源共享` CORS`
7. `withCredentials` 属性
8. `WebSocket` 协议跨域
9. `node` 代理跨域
10. `nginx` 代理跨域

### 2. HTTP 中与缓存相关的头部有哪些，它们有什么区别

|头部|优势和特点|劣势和问题|
|---|---|---|
|`Expires`|1、`HTTP 1.0` 产物，可以在`HTTP 1.0`和`1.1`中使用，简单易用。2、以时刻标识失效时间。|1、时间是由服务器发送的(`UTC`)，如果服务器时间和客户端时间存在不一致，可能会出现问题。2、存在版本问题，到期之前的修改客户端是不可知的。|
|`Cache-Control`|1、`HTTP 1.1` 产物，以时间间隔标识失效时间，解决了`Expires`服务器和客户端相对时间的问题。2、比`Expires`多了很多选项设置。|1、`HTTP 1.1 `才有的内容，不适用于`HTTP 1.0` 。2、存在版本问题，到期之前的修改客户端是不可知的。|
|`Last-Modified`|1、不存在版本问题，每次请求都会去服务器进行校验。服务器对比最后修改时间如果相同则返回304，不同返回200以及资源内容。|1、只要资源修改，无论内容是否发生实质性的变化，都会将该资源返回客户端。例如周期性重写，这种情况下该资源包含的数据实际上一样的。2、以时刻作为标识，无法识别一秒内进行多次修改的情况。3、某些服务器不能精确的得到文件的最后修改时间。|
|`ETag`|1、可以更加精确的判断资源是否被修改，可以识别一秒内多次修改的情况。2、不存在版本问题，每次请求都回去服务器进行校验。|	1、计算`ETag`值需要性能损耗。2、分布式服务器存储的情况下，计算`ETag`的算法如果不一样，会导致浏览器从一台服务器上获得页面内容后到另外一台服务器上进行验证时发现`ETag`不匹配的情况。|


### 3. 什么是强缓存，什么是协商缓存

浏览器第一次发起请求，本地有缓存情况：  在浏览器第一次发起请求时，本地无缓存，向web服务器发送请求，服务器起端响应请求，浏览器端缓存。过程如下：

![1](http://images.pingan8787.com/20190615http1.png)

在第一次请求时，服务器会将页面最后修改时间通过 `Last-Modified `标识由服务器发送给客户端，客户端记录修改时间；服务器还会生成一个Etag，并发送给客户端。

浏览器后续再次进行请求时：

![2](http://images.pingan8787.com/20190615http2.png)

浏览器缓存主要分为**强缓存**（也称本地缓存）和**协商缓存**（也称弱缓存）。

#### 3.1 强缓存

**强缓存**是利用 `http` 头中的 `Expires` 和 `Cache-Control` 两个字段来控制的，用来表示资源的缓存时间。强缓存中，普通刷新会忽略它，但不会清除它，需要强制刷新。浏览器强制刷新，请求会带上 `Cache-Control:no-cache` 和 `Pragma:no-cache`。   

#### 3.2 协商缓存

**协商缓存**就是由服务器来确定缓存资源是否可用，所以客户端与服务器端要通过某种标识来进行通信，从而让服务器判断请求资源是否可以缓存访问。    

普通刷新会启用弱缓存，忽略强缓存。只有在地址栏或收藏夹输入网址、通过链接引用资源等情况下，浏览器才会启用强缓存，这也是为什么有时候我们更新一张图片、一个js文件，页面内容依然是旧的，但是直接浏览器访问那个图片或文件，看到的内容却是新的。

这个主要涉及到两组 `header` 字段： `Etag` 和 `If-None-Match `、 `Last-Modified `和 `If-Modified-Since `。

### 4. 请简单介绍一下 LRU （Least recently used）算法

#### 4.1 原理

LRU（Least recently used，最近最少使用）算法根据数据的历史访问记录来进行淘汰数据，其核心思想是“如果数据最近被访问过，那么将来被访问的几率也更高”。
#### 4.2 实现
最常见的实现是使用一个链表保存缓存数据，详细算法实现如下：

1. 新数据插入到链表头部；

2. 每当缓存命中（即缓存数据被访问），则将数据移到链表头部；

3. 当链表满的时候，将链表尾部的数据丢弃。

#### 4.3 分析
* 命中率

当存在热点数据时，LRU的效率很好，但偶发性的、周期性的批量操作会导致LRU命中率急剧下降，缓存污染情况比较严重。

* 复杂度

实现简单。

* 代价

命中时需要遍历链表，找到命中的数据块索引，然后需要将数据移到头部。


### 参考：
1. [正确面对跨域，别慌](https://juejin.im/post/5a2f92c65188253e2470f16d#heading-20)

2. [HTTP 缓存的四种风味与缓存策略](https://segmentfault.com/a/1190000006689795)

3. [http协商缓存VS强缓存](https://segmentfault.com/a/1190000008956069)

4. [缓存淘汰算法--LRU算法](https://flychao88.iteye.com/blog/1977653)

5. [LRU算法](https://www.jianshu.com/p/d533d8a66795)