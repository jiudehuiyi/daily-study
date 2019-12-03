当用户输入URL后
[![769aa882-56a8-43a8-9999-2c0646d76da9](https://user-images.githubusercontent.com/25027560/39669427-0cb440e6-511e-11e8-9441-fa45a6beb33a.png)](https://user-images.githubusercontent.com/25027560/39669427-0cb440e6-511e-11e8-9441-fa45a6beb33a.png)

第一步就是DNS预解析一、原理

[域名系统 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/域名系统)
Domain Name System 将域名和IP地址相互映射的一个分布式数据库

DNS 预读取是一项使浏览器主动去执行域名解析的功能，其范围包括文档的所有链接，无论是图片的，CSS 的，还是 JavaScript 等其他用户能够点击的 URL，减少用户点击链接时的延迟。

当浏览器访问一个域名的时候，需要解析一次DNS，获得对应域名的ip地址。
浏览器缓存 => 系统缓存 => 路由器缓存 =>ISP(运营商)DNS缓存 => 根域名服务器 => 顶级域名服务器 => 主域名服务器的顺序
逐步读取缓存，直到拿到IP地址

作用：根据浏览器定义的规则，提前解析之后可能会用到的域名，使解析结果缓存到系统缓存中，缩短DNS解析时间，来提高网站的访问速度二、如何使用

### 1、打开和关闭DNS预解析

希望在`HTTPS`页面开启自动解析功能时，添加如下标记

```
<meta http-equiv="x-dns-prefetch-control" content="on">
// off 则是关闭
```

也可以通过在服务器端发送` X-DNS-Prefetch-Control` 报头

### 2、自动解析

Chromium会自动解析`href`属性（a标签），该行为与用户浏览网页是并行的。但为了确保安全，`HTTPS`页面不会自动解析
[DNS Prefetching - Chromium官方文档](https://www.chromium.org/developers/design-documents/dns-prefetching)

> Chromium不使用浏览器的网络堆栈，直接使用操作系统的缓存。通过8个异步线程执行预解析，每个线程处理一个队列，来等待域名的响应，最终操作系统会响应一个DNS结果给线程，然后线程丢弃它，等待下一个预解析请求。

### 3、手动添加解析

```
<link rel="dns-prefetch" href="http://www.google.com">
```

### 4、在浏览器中设置

一般来说并不需要去管理预读取，但是可能会有用户希望关闭预读取功能。这时只需要设置` network.dns.disablePrefetch preference` 值为 `true` 就可以了

默认情况下，通过 `HTTPS` 加载的页面上内嵌链接的域名并不会执行预加载。在 Firefox 浏览器中，可以通过设置 `network.dns.disablePrefetchFromHTTPS` 值为 `false` 来改变这一默认行为。三、看看淘宝的DNS预解析

DNS Prefetch 的原理就是在 HTTP 建立之前，将 DNS 查询的结果缓存到系统/浏览器中，提升网页的加载效率

让我们来实际看一下淘宝的DNS预解析是怎么做的

进[WebPagetest](http://www.webpagetest.org/)输入`https://www.taobao.com`打开淘宝，看它的link标签，带`rel='dns-prefetch'`的那些
然后应该可以发现，上面那些解析时间比较长的域名没有在这个列表中
[![36050f3d-5c7b-45ca-b7be-732655043174](https://user-images.githubusercontent.com/25027560/39669432-1c868b8c-511e-11e8-932d-537f87ae0e8f.png)](https://user-images.githubusercontent.com/25027560/39669432-1c868b8c-511e-11e8-932d-537f87ae0e8f.png)
找到结果中的下面内容，看`DNS Lookup`一项
那些结果比较大的，就是没有预解析的
[![24d9d988-a586-4561-944a-b302e9204987](https://user-images.githubusercontent.com/25027560/39669431-178caae4-511e-11e8-82d9-07e61498f4e1.png)](https://user-images.githubusercontent.com/25027560/39669431-178caae4-511e-11e8-82d9-07e61498f4e1.png)

## 四、使用场景

1、新用户访问，后端可以通过 Cookie 判断是否为首次进入站点，对于这类用户，DNS Prefetch 可以比较明显地提升访问速度
2、登录页，提前在页面上进行下一跳页用到资源的 DNS Prefetch
2、上面说到chrome使用了8个异步线程来处理DNS预解析，所以过多的prefetch并不一定能提高网页加载效率

## 五、如何更好的使用？

### 1、对哪些资源做手动prefetch

1、静态资源域名
2、JS里会发起跳转的域名
3、会重定向的域名

### 2、不用对超链接做手动prefetch，浏览器会自动做

### 3、手动DNS预解析还可以优化吗？

实际上还是会增加html的代码量的，特别是域名多的情况下
可以通过js初始化一个iframe异步加载一个页面，而这个页面里包含本站所有的需要手动dns prefetching的域名

### 4、页面Head里面有个css链接, 在当前页的Head里加上对应的手动dns prefetching的link标签，实际上并没有好处

## 六、域名发散和域名收敛

[![319ec85e-2bea-42b8-9dc9-02a523aa6bd8](https://user-images.githubusercontent.com/25027560/46642944-5a9f4180-cbac-11e8-838b-b2cb5c0bb99a.png)](https://user-images.githubusercontent.com/25027560/46642944-5a9f4180-cbac-11e8-838b-b2cb5c0bb99a.png)

### 1、域名发散

在PC上，为了突破浏览器的单域名多线程并发限制，大家会采用域名发散：http 静态资源采用多个子域名，以提供最大并行度，让客户端加载静态资源更为迅速

为什么浏览器要做并发限制？
1、以前的服务器负载能力差，流量大容易奔溃，所以为了保护服务器，浏览器做了单域名最大并发限制
2、防止DDOS攻击，最基本的 DoS 攻击就是利用合理的服务请求来占用过多的服务资源，从而使合法用户无法得到服务的响应。如果不限制并发请求数量…

### 2、域名收敛

顾名思义：尽量将静态资源只放在一个域名下面

既然域名发散优点这么明显，那么域名收敛怎么来的？

上面说了是PC下使用域名发散，那么现在是移动互联网时代，无线设备占多（写这个文章的时候是2018年末，5G都快来了，地域和网速限制马上不会再成为瓶颈，但还是要究其根本）

首先，HTTP请求需要经历这么个过程
DNS域名解析 -> TCP 3次握手 -> 发起HTTP请求 -> 服务器响应HTTP请求 -> …… -> 浏览器解析渲染页面

第一个DNS解析是一个很复杂的过程，此处略过…

PC上DNS解析消耗相对较小
但移动端（假设信号不够强）的DNS消耗是比较可观的（相对而言）
所以，在增加域名的同时，会带来一定的DNS解析消耗，所以域名收敛能降低这个成本。

但是，单纯的靠域名收敛降低这个成本，貌似对性能提升是个鸡肋

那么单域名的并发问题还是存在，怎么处理，核心是解除最大连接数的限制，那么`SPDY/HTTP2`的多路复用功能就派上用场了

这两新协议对HTTP1.1做了不少的优化，核心是减少连接数，还有头部压缩、服务器推送，强制SSL安全协议等等

总的来说，尽快拥抱新技术吧