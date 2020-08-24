---
categories:
    - HTTP网络协议
tags:
    - HTTP网络协议
date: 2020-08-24
---

HTTP 起源

`HTTP` 是由蒂姆·伯纳斯-李（`TimBerners—Lee`）于1989年在欧洲核子研究组织（`CERN`）所发起

其中最著名的是 1999 年 6 月公布的 [RFC 2616](https://tools.ietf.org/html/rfc2616)，定义了 `HTTP` 协议中现今广泛使用的一个版本——`HTTP 1.1`



![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/61d828afb4494d9fba43bbd1f7ff69f5~tplv-k3u1fbpfcp-zoom-1.image)



## HTTP 是什么

全称：超文本传输协议（`HyperText Transfer Protocol`）

概念：`HTTP` 是一种能够获取像 `HTML`、图片等网络资源的通讯协议（`protocol`）。它是在 `web` 上进行数据交换的基础，是一种 `client-server` 协议

`HTTP`——因特网的多媒体信使 ——《HTTP权威指南》。 `HTTP` 在因特网的角色：充当一个信使的角色，干的就是一个跑腿的活，在客户端和服务端之间传递信息，但我们又不能缺少它。`HTTP` 协议是应用层的协议，是与前端开发最息息相关的协议。平时我们遇到的 `HTTP` 请求、 `HTTP` 缓存、`Cookies`、跨域等其实都跟 `HTTP` 息息相关

## HTTP 的基础特性

- 可拓展协议。`HTTP 1.0` 出现的 `HTTP headers` 让协议拓展变得更加的容易。只要服务端和客户端就 `headers` 达成语义一致，新功能就可以被轻松的加入进来
- `HTTP` 是无状态的、有会话的。在同一个连接中，两个执行成功的 `HTTP` 请求之间是没有关系的。这就带来了一个问题，用户没有办法在同一个网站中进行连续的交互，比如在一个电商网站里，用户把某个商品加入到购物车，切换一个页面后再次添加了商品，这两次添加商品的请求之间没有关联，浏览器无法知道用户最终选择了哪些商品。而使用 `HTTP` 的头部扩展，`HTTP Cookies` 就可以解决这个问题。把 `Cookies` 添加到头部中，创建一个会话让每次请求都能共享相同的上下文信息，达成相同的状态。
- `HTTP` 与连接。通过 `TCP`，或者 `TLS`——加密的 `TCP` 连接来发送，理论上任何可靠的传输协议都可以使用。连接是传输层控制的，这从根本上来讲不是 `HTTP` 的范畴。



![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f87021221c4a4e0f8fbf7704366d68a7~tplv-k3u1fbpfcp-zoom-1.image)



也就是说，`HTTP` 依赖于面向连接的 `TCP` 进行消息传递，但连接并不是必须的。只需要它是可靠的，或不丢失消息的（至少返回错误）。

`HTTP/1.0` 默认为每一对 `HTTP` 请求/响应都打开一个单独的 `TCP` 连接。当需要连续发起多个请求时，这种模式比多个请求共享同一个 `TCP` 链接更低效。为此，`HTTP 1.1` 持久连接的概念，底层 `TCP` 连接可以通过 `connection` 头部实现。但 `HTTP 1.1` 在连接上也是不完美的，后面我们会提到。

## 基于 HTTP 的组件系统

`HTTP` 的组件系统包括客户端、`web` 服务器和代理



![image](https://user-images.githubusercontent.com/20135760/90960848-1b93e400-e4d7-11ea-88ee-667fd4ef1c47.png)



### 客户端：user-agent

浏览器，特殊比如是工程师使用的程序，以及 `Web` 开发人员调试应用程序

### Web服务端

由 `Web Server` 来服务并提供客户端所请求的文档。每一个发送到服务器的请求，都会被服务器处理并返回一个消息，也就是 `response`

### 代理（Proxies）

在浏览器和服务器之间，有很多计算机和其他设备转发了 `HTTP` 消息。它们可能出现在传输层、网络层和物理层上，对于 `HTTP` 应用层而言就是透明的

有如下的一些作用

- 缓存
- 过滤（像防病毒扫描、家长控制）
- 负载均衡
- 认证（对不同的资源进行权限控制）
- 日志管理

## HTTP 报文组成

HTTP 有两种类型的消息：

- 请求——由客户端发送用来触发一个服务器上的动作
- 响应——来自服务器端的应答

`HTTP` 消息由采用 `ASCII` 编码的多行文本构成的。在 `HTTP/1.1` 以及更早的版本中，这些消息通过连接公开的发送。在 `HTTP2.0` 中，消息被分到了多个 `HTTP` 帧中。通过配置文件（用于代理服务器或者服务器），`API`（用于浏览器）或者其他接口提供 `HTTP` 消息

### 典型的 HTTP 会话

- 建立连接 在客户端-服务器协议中，连接是由客户端发起建立的。在 `HTTP` 中打开连接意味着在底层传输层启动连接，通常是 `TCP`。使用 `TCP` 时，`HTTP` 服务器的默认端口号是 `80`，另外还有 `8000` 和 `8080` 也很常用
- 发送客户端请求
- 服务器响应请求

### HTTP 请求和响应

HTTP 请求和响应都包括起始行（`start line`）、请求头（`HTTP Headers`）、空行（`empty line`）以及 `body` 部分，如下图所示：



![image](https://user-images.githubusercontent.com/20135760/90958376-a8827180-e4c6-11ea-895a-08c8852ef77c.png)



- 起始行。 请求的起始行：请求方法、请求 `Path` 和`HTTP` 版本号 响应的起始行：`HTTP` 版本号、响应状态码以及状态文本描述

下面详细说下请求 `Path`，请求路径（`Path`）有以下几种：

1）一个绝对路径，末尾跟上一个 ' ? ' 和查询字符串。这是最常见的形式，称为 原始形式 (`origin form`)，被 `GET`，`POST`，`HEAD` 和 `OPTIONS` 方法所使用

```http
POST / HTTP/1.1
GET /background.png HTTP/1.0
HEAD /test.html?query=alibaba HTTP/1.1
OPTIONS /anypage.html HTTP/1.0
```

2）一个完整的 `URL`。主要在使用 `GET` 方法连接到代理的时候使用

```http
GET http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages HTTP/1.1
```

3）由域名和可选端口（以':'为前缀）组成的 `URL` 的 `authority component`，称为 `authority form`。仅在使用 `CONNECT` 建立 `HTTP` 隧道时才使用

```http
CONNECT developer.mozilla.org:80 HTTP/1.1
```

4）星号形式 (`asterisk form`)，一个简单的星号('*')，配合 `OPTIONS` 方法使用，代表整个服务器。 

```http
OPTIONS * HTTP/1.1
```

- `Headers` 请求头或者响应头。详见下面的首部。 不区分大小写的字符串，紧跟着的冒号 (':') 和一个结构取决于 `header` 的值
- 空行。很多人容易忽略
- `Body`

请求 `Body` 部分： 有些请求将数据发送到服务器以便更新数据：常见的的情况是 `POST` 请求（包含 `HTML` 表单数据）。请求报文的 `Body` 一般为两类。一类是通过 `Content-Type` 和 `Content-Length` 定义的单文件 `body`。另外一类是由多 `Body` 组成，通常是和 `HTML Form` 联系在一起的。两者的不同表现在于 `Content-Type`的值。

1）`Content-Type —— application/x-www-form-urlencoded` 对于 `application/x-www-form-urlencoded` 格式的表单内容，有以下特点:

  I.其中的数据会被编码成以&分隔的键值对

  II.字符以URL编码方式编码。

```
// 转换过程: {a: 1, b: 2} -> a=1&b=2 -> 如下(最终形式)
"a%3D1%26b%3D2"
```

2)`Content-Type —— multipart/form-data`

请求头中的 `Content-Type` 字段会包含 `boundary`，且 `boundary` 的值有浏览器默认指定。例: `Content-Type: multipart/form-data;boundary=----WebkitFormBoundaryRRJKeWfHPGrS4LKe`。

数据会分为多个部分，每两个部分之间通过分隔符来分隔，每部分表述均有 `HTTP` 头部描述子包体，如`Content-Type`，在最后的分隔符会加上--表示结束。

```http
Content-Disposition: form-data;name="data1";
Content-Type: text/plain
data1
----WebkitFormBoundaryRRJKeWfHPGrS4LKe
Content-Disposition: form-data;name="data2";
Content-Type: text/plain
data2
----WebkitFormBoundaryRRJKeWfHPGrS4LKe--
```

响应 `Body` 部分：

1）由已知长度的单个文件组成。该类型 `body` 由两个 `header` 定义：`Content-Type` 和 `Content-Length`

2）由未知长度的单个文件组成，通过将 `Transfer-Encoding` 设置为 `chunked` 来使用 `chunks` 编码。

关于 `Content-Length` 在下面 `HTTP 1.0` 中会提到，这个是 `HTTP 1.0` 中新增的非常重要的头部。

### 方法

安全方法：`HTTP` 定义了一组被称为安全方法的方法。`GET` 方法和 `HEAD` 方法都被认为是安全的，这意味着 `GET` 方法和 `HEAD` 方法都不会产生什么动作 —— `HTTP` 请求不会再服务端产生什么结果，但这并不意味着什么动作都没发生，其实这更多的是 `web` 开发者决定的

- `GET`：请求服务器发送某个资源
- `HEAD`：跟 `GET` 方法类似，但服务器在响应中只返回了首部。不会返回实体的主体部分。
- `PUT`：向服务器中写入文档。语义：用请求的主体部分来创建一个由所请求的 `URL` 命名的新文档
- `POST`：用来向服务器中输入数据的。通常我们提交表单数据给服务器。【`POST` 用于向服务器发送数据，`PUT` 方法用于向服务器上的资源（例如文件）中存储数据】
- `TRACE`：主要用于诊断。实现沿通向目标资源的路径的消息环回（`loop-back`）测试 ，提供了一种实用的 `debug` 机制。
- `OPTIONS`：请求 `WEB` 服务器告知其支持的各种功能。可以询问服务器支持哪些方法。或者针对某些特殊资源支持哪些方法。
- `DELETE`：请求服务器删除请求 `URL` 中指定的的资源

### GET 和 POST 的区别

首先要了解下副作用和幂等的概念，副作用指的是对服务器端资源做修改。幂等指发送 `M` 和 `N` 次请求（两者不相同且都大于 1），服务器上资源的状态一致。应用场景上，get是无副作用的，幂等的。post 主要是有副作用的，不幂等的情况

技术上有以下的区分：

- 缓存：`Get` 请求能缓存，`Post` 请求不能
- 安全：`Get` 请求没有 `Post` 请求那么安全，因为请求都在 `URL` 中。且会被浏览器保存历史纪录。`POST` 放在请求体中，更加安全
- 限制：`URL` 有长度限制，会干预 `Get` 请求，这个是浏览器决定的
- 编码：`GET` 请求只能进行 `URL` 编码，只能接收 `ASCII` 字符，而 `POST` 没有限制。`POST` 支持更多的编码类型，而且不对数据类型做限制
- 从 `TCP` 的角度，`GET` 请求会把请求报文一次性发出去，而 `POST` 会分为两个 `TCP` 数据包，首先发 `header` 部分，如果服务器响应 `100(continue)`， 然后发 `body` 部分。(火狐浏览器除外，它的 `POST` 请求只发一个 `TCP` 包)

### 状态码

- 100~199——信息性状态码

  101 Switching Protocols。在HTTP升级为WebSocket的时候，如果服务器同意变更，就会发送状态码 101。

- 200~299——成功状态码

  200 OK，表示从客户端发来的请求在服务器端被正确处理

  204 No content，表示请求成功，但响应报文不含实体的主体部分

  205 Reset Content，表示请求成功，但响应报文不含实体的主体部分，但是与 204 响应不同在于要求请求方重置内容

  206 Partial Content，进行范围请求

- 300~399——重定向状态码

  301 moved permanently，永久性重定向，表示资源已被分配了新的 URL

  302 found，临时性重定向，表示资源临时被分配了新的 URL

  303 see other，表示资源存在着另一个 URL，应使用 GET 方法获取资源

  304 not modified，表示服务器允许访问资源，但因发生请求未满足条件的情况

  307 temporary redirect，临时重定向，和302含义类似，但是期望客户端保持请求方法不变向新的地址发出请求

- 400~499——客户端错误状态码

  400 bad request，请求报文存在语法错误

  401 unauthorized，表示发送的请求需要有通过 HTTP 认证的认证信息

  403 forbidden，表示对请求资源的访问被服务器拒绝

  404 not found，表示在服务器上没有找到请求的资源 

- 500~599——服务器错误状态码

  500 internal sever error，表示服务器端在执行请求时发生了错误

  501 Not Implemented，表示服务器不支持当前请求所需要的某个功能

  503 service unavailable，表明服务器暂时处于超负载或正在停机维护，无法处理请求

### 首部

`HTTP Headers`

1.通用首部（`General headers`）同时适用于请求和响应消息，但与最终消息主体中传输的数据无关的消息头。如 `Date`

2.请求首部（`Request headers`）包含更多有关要获取的资源或客户端本身信息的消息头。如 User-Agent

3.响应首部（`Response headers`）包含有关响应的补充信息

4.实体首部（`Entity headers`）含有关实体主体的更多信息，比如主体长(`Content-Length`)度或其 `MIME` 类型。如 `Accept-Ranges`

详细的 `Header` 见 [HTTP Headers 集合](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)

## HTTP 的前世今生

`HTTP（HyperText Transfer Protocol）`是万维网（`World Wide Web`）的基础协议。`Tim Berners-Lee` 博士和他的团队在`1989-1991`年间创造出它。【HTTP、网络浏览器、服务器】

在 1991 年发布了 `HTTP 0.9` 版，在 1996 年发布 1.0 版，1997 年是 1.1 版，1.1 版也是到今天为止传输最广泛的版本。2015 年发布了 2.0 版，其极大的优化了 `HTTP/1.1` 的性能和安全性，而 2018 年发布的 3.0 版，继续优化 `HTTP/2`，激进地使用 `UDP` 取代 `TCP` 协议，目前，`HTTP/3` 在 2019 年 9 月 26 日 被 `Chrome`，`Firefox`，和 `Cloudflare` 支持



![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f52c8c0c2df647528c783e1011ea14b5~tplv-k3u1fbpfcp-zoom-1.image)



### HTTP 0.9

单行协议，请求由单行指令构成。以唯一可用的方法 `GET` 开头。后面跟的是目标资源的路径

```http
GET /mypage.html
```

响应：只包括响应文档本身

```html
<HTML>
这是一个非常简单的HTML页面
</HTML>
```

- 没有响应头，只传输 `HTML` 文件
- 没有状态码

### HTTP 1.0

[RFC 1945](https://tools.ietf.org/html/rfc1945) 提出了 `HTTP1.0`，**构建更好可拓展性**

- 协议版本信息会随着每个请求发送
- 响应状态码
- 引入了 `HTTP` 头的概念，无论是请求还是拓展，允许传输元数据。使协议变得灵活，更加具有拓展性
- `Content-Type` 请求头，具备了传输除纯文本 `HTML` 文件以外其他类型文档的能力 在响应中，`Content-Type` 标头告诉客户端实际返回的内容的内容类型

媒体类型是一种标准。用来表示文档、文件或者字节流的性质和格式。浏览器通常使用 `MIME` （`Multipurpose Internet Mail Extensions` ）类型来确定如何处理 `URL`，因此 `Web` 服务器在响应头中配置正确的 `MIME` 类型会非常的重要。如果配置不正确，可能会导致网站无法正常的工作。`MIME` 的组成结构非常简单；由类型与子类型两个字符串中间用'/'分隔而组成。

`HTTP` 从 `MIME type` 取了一部分来标记报文 `body` 部分的数据类型，这些类型体现在`Content-Type` 这个字段，当然这是针对于发送端而言，接收端想要收到特定类型的数据，也可以用 `Accept` 字段。

这两个字段的取值可以分为下面几类:

```
- text： text/html, text/plain, text/css 等
- image: image/gif, image/jpeg, image/png 等
- audio/video: audio/mpeg, video/mp4 等
- application: application/json, application/javascript, application/pdf, application/octet-stream

```

同时为了约定请求的数据和响应数据的压缩方式、支持语言、字符集等，还提出了以下的 `Header`

1.压缩方式:发送端：`Content-Encoding`（服务端告知客户端，服务器对实体的主体部分的编码方式） 和 接收端：`Accept-Encoding`（用户代理支持的编码方式），值有 gzip: 当今最流行的压缩格式；deflate: 另外一种著名的压缩格式；br: 一种专门为 HTTP 发明的压缩算法

2.支持语言：`Content-Language` 和 `Accept-Language`（用户代理支持的自然语言集）

3.字符集：发送端：`Content-Type` 中，以 `charset` 属性指定。接收端： `Accept-Charset`（用户代理支持的字符集）。

```http
// 发送端
Content-Encoding: gzip
Content-Language: zh-CN, zh, en
Content-Type: text/html; charset=utf-8

// 接收端
Accept-Encoding: gzip
Accept-Language: zh-CN, zh, en
Accept-Charset: charset=utf-8
```

虽然  `HTTP1.0` 在 `HTTP 0.9` 的基础上改进了很多，但还是存在这不少的缺点

`HTTP/1.0` 版的主要缺点是，每个 `TCP` 连接只能发送一个请求。发送数据完毕，连接就关闭，如果还要请求其他资源，就必须再新建一个连接。 `TCP` 连接的新建成本很高，因为需要客户端和服务器三次握手，并且开始时发送速率较慢（`slow start`）。

`HTTP` 最早期的模型，也是  `HTTP/1.0` 的默认模型，是短连接。每一个 `HTTP` 请求都由它自己独立的连接完成；这意味着发起每一个 `HTTP` 请求之前都会有一次 `TCP` 握手，而且是连续不断的。

### HTTP 1.1

`HTTP/1.1` 在1997年1月以 [RFC 2068](https://tools.ietf.org/html/rfc2068) 文件发布。

`HTTP 1.1` 消除了大量歧义内容并引入了多项技术

- 连接可以复用。长连接：`connection: keep-alive`。`HTTP 1.1` 支持长连接（`PersistentConnection`），在一个 `TCP` 连接上可以传送多个 `HTTP` 请求和响应，减少了建立和关闭连接的消耗和延迟，在 `HTTP1.1` 中默认开启 `Connection： keep-alive`，一定程度上弥补了 `HTTP1.0` 每次请求都要创建连接的缺点。
- 增加了管道化技术（`HTTP Pipelinling`），允许在第一个应答被完全发送完成之前就发送第二个请求，以降低通信延迟。复用同一个 `TCP` 连接期间，即便是通过管道同时发送了多个请求，服务端也是按请求的顺序依次给出响应的；而客户端在未收到之前所发出所有请求的响应之前，将会阻塞后面的请求(排队等待)，这称为"队头堵塞"（`Head-of-line blocking`）。
- 支持响应分块，分块编码传输：`Transfer-Encoding: chunked` `Content-length` 声明本次响应的数据长度。`keep-alive` 连接可以先后传送多个响应，因此用 `Content-length` 来区分数据包是属于哪一个响应。 使用 `Content-Length` 字段的前提条件是，服务器发送响应之前，必须知道响应的数据长度。 对于一些很耗时的动态操作来说，这意味着，服务器要等到所有操作完成，才能发送数据，显然这样的效率不高。更好的处理方法是，产生一块数据，就发送一块，采用"流模式"（`Stream`）取代"缓存模式"（`Buffer`）。因此，`HTTP 1.1` 规定可以不使用 `Content-Length` 字段，而使用"分块传输编码"（`Chunked Transfer Encoding`）。只要请求或响应的头信息有 `Transfer-Encoding: chunked` 字段，就表明 `body` 将可能由数量未定的多个数据块组成。 每个数据块之前会有一行包含一个 16 进制数值，表示这个块的长度；最后一个大小为 0 的块，就表示本次响应的数据发送完了。
- 引入额外的缓存控制机制。在 `HTTP1.0` 中主要使用 `header` 里的 `If-Modified-Since`,`Expires` 等来做为缓存判断的标准，`HTTP1.1` 则引入了更多的缓存控制策略例如 `Entity tag`, `If-None-Match`，`Cache-Control` 等更多可供选择的缓存头来控制缓存策略。
- `Host` 头。不同的域名配置同一个 `IP` 地址的服务器。`Host` 是 `HTTP 1.1` 协议中新增的一个请求头，主要用来实现虚拟主机技术。

虚拟主机（`virtual hosting`）即共享主机（`shared web hosting`），可以利用虚拟技术把一台完整的服务器分成若干个主机，因此可以在单一主机上运行多个网站或服务。

举个栗子，有一台 `ip` 地址为 `61.135.169.125` 的服务器，在这台服务器上部署着谷歌、百度、淘宝的网站。为什么我们访问 `https://www.google.com` 时，看到的是 `Google` 的首页而不是百度或者淘宝的首页？原因就是 `Host` 请求头决定着访问哪个虚拟主机。

### HTTP 2.0

2015年，`HTTP2.0` 面世。

- `HTTP/2`是二进制协议而不是文本协议。先来看几个概念：
  - 帧：客户端与服务器通过交换帧来通信，帧是基于这个新协议通信的最小单位。
  - 消息：是指逻辑上的 HTTP 消息，比如请求、响应等，由一或多个帧组成。
- 流：流是连接中的一个虚拟信道，可以承载双向的消息；每个流都有一个唯一的整数标识符

`HTTP 2.0` 中的帧将 `HTTP/1.x` 消息分成帧并嵌入到流 (`stream`) 中。数据帧和报头帧分离，这将允许报头压缩。将多个流组合，这是一个被称为多路复用 (`multiplexing`) 的过程，它允许更有效的底层 `TCP` 连接。

也就是说，流用来承载消息，消息又是有一个或多个帧组成。二进制传输的方式更加提升了传输性能。 每个数据流都以消息的形式发送，而消息又由一个或多个帧组成。 帧是流中的数据单位。

`HTTP` 帧现在对 `Web` 开发人员是透明的。在 `HTTP/2` 中，这是一个在  `HTTP/1.1` 和底层传输协议之间附加的步骤。`Web` 开发人员不需要在其使用的 `API` 中做任何更改来利用 `HTTP` 帧；当浏览器和服务器都可用时，`HTTP/2` 将被打开并使用。



![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c9586347bbb04e379f071cf44af7b339~tplv-k3u1fbpfcp-zoom-1.image)



- 这是一个复用协议。并行的请求能在同一个连接中处理，移除了 `HTTP/1.x` 中顺序和阻塞的约束。多路复用允许同时通过单一的 `HTTP/2`  连接发起多重的请求-响应消息

之前我们提到，虽然 `HTTP 1.1` 有了长连接和管道化的技术，但是还是会存在 队头阻塞。而 `HTTP 2.0` 就解决了这个问题 `HTTP/2` 中新的二进制分帧层突破了这些限制，实现了完整的请求和响应复用：客户端和服务器可以将 `HTTP` 消息分解为互不依赖的帧，然后交错发送，最后再在另一端把它们重新组装起来。



![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e464e3d3f47740568e25d64fe9e04e38~tplv-k3u1fbpfcp-zoom-1.image)



如上图所示，快照捕捉了同一个连接内并行的多个数据流。 客户端正在向服务器传输一个 `DATA` 帧（数据流 5），与此同时，服务器正向客户端交错发送数据流 1 和数据流 3 的一系列帧。因此，一个连接上同时有三个并行数据流。

将 HTTP 消息分解为独立的帧，交错发送，然后在另一端重新组装是 `HTTP 2` 最重要的一项增强。事实上，这个机制会在整个网络技术栈中引发一系列连锁反应，从而带来巨大的性能提升，让我们可以： 1.并行交错地发送多个请求，请求之间互不影响。 2.并行交错地发送多个响应，响应之间互不干扰。 3.使用一个连接并行发送多个请求和响应。 4.消除不必要的延迟和提高现有网络容量的利用率，从而减少页面加载时间。 5.不必再为绕过 HTTP/1.x 限制而做很多工作(比如精灵图) ...

连接共享，即每一个 `request` 都是是用作连接共享机制的。一个 `request` 对应一个 `id`，这样一个连接上可以有多个 `request`，每个连接的 `request` 可以随机的混杂在一起，接收方可以根据 `request` 的 `id` 将 `request` 再归属到各自不同的服务端请求里面。

`HTTP 1.1` 和 `HTTP 2.0` 的对比，可以参考这个 [网站 demo 演示](https://http2.akamai.com/demo)

`HTTP 1.1` 演示如下： 

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/53f8c657e9c0460b9cd64e3e15b73f37~tplv-k3u1fbpfcp-zoom-1.image)



`HTTP2.0` 演示如下：



![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a241d177971a47c89843f757068a9fa6~tplv-k3u1fbpfcp-zoom-1.image)





![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/63e10d6481ea4a68b6c7fb6e392f3eb1~tplv-k3u1fbpfcp-zoom-1.image)



- 压缩了`headers`。`HTTP1.x` 的 `header` 带有大量信息，而且每次都要重复发送，就造成了性能的损耗。 为了减少此开销和提升性能，`HTTP/2` 使用 `HPACK` 压缩格式压缩请求和响应标头元数据，这种格式采用两种简单但是强大的技术： 这种格式支持通过静态霍夫曼代码对传输的标头字段进行编码，从而减小了各个传输的大小。 这种格式要求客户端和服务器同时维护和更新一个包含之前见过的标头字段的索引列表（换句话说，它可以建立一个共享的压缩上下文），此列表随后会用作参考，对之前传输的值进行有效编码。



![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ad8f1dbe0b74f6d80e84c5aaef7953d~tplv-k3u1fbpfcp-zoom-1.image)



- 服务端推送。其允许服务器在客户端缓存中填充数据，通过一个叫服务器推送的机制来提前请求。服务器向客户端推送资源无需客户端明确地请求，服务端可以提前给客户端推送必要的资源，这样可以减少请求延迟时间，例如服务端可以主动把 `JS` 和 `CSS` 文件推送给客户端，而不是等到 `HTML` 解析到资源时发送请求，这样可以减少延迟时间大致过程如下图所示：



![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/346e4b46c3de44c99deca80c143948e5~tplv-k3u1fbpfcp-zoom-1.image)



### 如何升级你的 HTTP 版本

使用 `HTTP/1.1` 和 `HTTP/2` 对于站点和应用来说是透明的。拥有一个最新的服务器和新点的浏览器进行交互就足够了。只有一小部分群体需要做出改变，而且随着陈旧的浏览器和服务器的更新，而不需 `Web` 开发者做什么，用的人自然就增加了

## HTTPS

`HTTPS` 也是通过 `HTTP` 协议进行传输信息，但是采用了 `TLS` 协议进行了加密

### 对称加密和非对称加密

对称加密就是两边拥有相同的秘钥，两边都知道如何将密文加密解密。但是因为传输数据都是走的网络，如果将秘钥通过网络的方式传递的话，一旦秘钥被截获就没有加密的意义的

非对称加密

公钥大家都知道，可以用公钥加密数据。但解密数据必须使用私钥，私钥掌握在颁发公钥的一方。首先服务端将公钥发布出去，那么客户端是知道公钥的。然后客户端创建一个秘钥，并使用公钥加密，发送给服务端。服务端接收到密文以后通过私钥解密出正确的秘钥

### TLS 握手过程

`TLS` 握手的过程采用的是非对称加密

- `Client Hello`: 客户端发送一个随机值(`Random1`)以及需要的协议和加密方式。
- `Server Hello` 以及 `Certificate`: 服务端收到客户端的随机值，自己也产生一个随机值(`Random2`)，并根据客户端需求的协议和加密方式来使用对应的方式，并且发送自己的证书（如果需要验证客户端证书需要说明）
- `Certificate Verify`: 客户端收到服务端的证书并验证是否有效，验证通过会再生成一个随机值(`Random3`)，通过服务端证书的公钥去加密这个随机值并发送给服务端，如果服务端需要验证客户端证书的话会附带证书
- `Server 生成 secret`: 服务端收到加密过的随机值并使用私钥解密获得第三个随机值(`Random3`)，这时候两端都拥有了三个随机值，可以通过这三个随机值按照之前约定的加密方式生成密钥，接下来的通信就可以通过该密钥来加密解密

## HTTP 缓存

### 强缓存

强缓存主要是由 `Cache-control` 和 `Expires` 两个 `Header` 决定的

`Expires` 的值和头里面的 `Date` 属性的值来判断是否缓存还有效。`Expires` 是 `Web` 服务器响应消息头字段，在响应 `http` 请求时告诉浏览器在过期时间前浏览器可以直接从浏览器缓存取数据，而无需再次请求。`Expires` 的一个缺点就是，返回的到期时间是服务器端的时间，这是一个绝对的时间，这样存在一个问题，如果客户端的时间与服务器的时间相差很大（比如时钟不同步，或者跨时区），那么误差就很大。

`Cache-Control` 指明当前资源的有效期，控制浏览器是否直接从浏览器缓存取数据还是重新发请求到服务器取数据。但是其设置的是一个相对时间。

指定过期时间：`max-age` 是距离请求发起的时间的秒数，比如下面指的是距离发起请求 31536000S 内都可以命中强缓存

```http
Cache-Control: max-age=31536000
```

表示没有缓存

```http
Cache-Control: no-store
```

有缓存但要重新验证

```http
Cache-Control: no-cache
```

私有和公共缓存

`public` 表示响应可以被任何中间人（比如中间代理、`CDN` 等缓存） 而 `private` 则表示该响应是专用于某单个用户的，中间人不能缓存此响应，该响应只能应用于浏览器私有缓存中。

```http
Cache-Control: private
Cache-Control: public
```

验证方式：以下表示一旦资源过期（比如已经超过 `max-age`），在成功向原始服务器验证之前，缓存不能用该资源响应后续请求

```http
Cache-Control: must-revalidate
```

`Cache-control` 优先级比 `Expires` 优先级高

以下是一个 `Cache-Control` 强缓存的过程：

- 首次请求，直接从 server 中获取。其中会设置 `max-age=100`
- 第二次请求，`age=10`，小于 100，则命中 `Cache`，直接返回
- 第三次请求，`age=110`，大于 110。强缓存失效，就需要再次请求 `Server`



![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/24a4507dfd0c4163875cd28a74f7643d~tplv-k3u1fbpfcp-zoom-1.image)



### 协商缓存

- `If-Modified-Since——Last-Modified`

`Last-Modified` 表示本地文件最后修改日期，浏览器会在 `request header` 加上 `If-Modified-Since`（上次返回的 `Last-Modified` 的值），询问服务器在该日期后资源是否有更新，有更新的话就会将新的资源发送回来

但是如果在本地打开缓存文件，就会造成 `Last-Modified` 被修改，所以在 `HTTP / 1.1` 出现了 `ETag`

- `If-none-match——ETags`

`Etag` 就像一个指纹，资源变化都会导致 `ETag` 变化，跟最后修改时间没有关系，`ETag` 可以保证每一个资源是唯一的。`If-None-Match` 的 `header` 会将上次返回的 `Etag` 发送给服务器，询问该资源的 `Etag` 是否有更新，有变动就会发送新的资源回来

```
If-none-match`、`ETags` 优先级高于 `If-Modified-Since、Last-Modified
```

第一次请求：



![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0da1e104a3ae4eb0a79f17ad8248d186~tplv-k3u1fbpfcp-zoom-1.image)



第二次请求相同网页：



![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/576ffbc828c5474cac1b11f4ddb89780~tplv-k3u1fbpfcp-zoom-1.image)



协商缓存，假如没有改动的话，返回 304 ，改动了返回 200 资源

- 200：强缓存 `Expires/Cache-Control` 失效时，返回新的资源文件
- 200 `(from cache)`: 强缓 `Expires/Cache-Control` 两者都存在，未过期，`Cache-Control` 优先 `Expires` 时，浏览器从本地获取资源成功
- 304 `(Not Modified)`：协商缓存 `Last-modified/Etag` 没有过期时，服务端返回状态码304

现在的200`(from cache)`已经变成了 `disk cache`(磁盘缓存)和 `memory cache`(内存缓存)两种



![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/54857c38d0ef478cbd3e39f903eb6e65~tplv-k3u1fbpfcp-zoom-1.image)



### revving 技术

上面提到 `HTTP` 缓存相关，但是很多有时候，我们希望上线之后需要更新线上资源。

`web` 开发者发明了一种被 `Steve Souders` 称之为 `revving` 的技术。不频繁更新的文件会使用特定的命名方式：在 `URL` 后面（通常是文件名后面）会加上版本号。

弊端：更新了版本号，所有引用这些的资源的地方的版本号都要改变

`web` 开发者们通常会采用自动化构建工具在实际工作中完成这些琐碎的工作。当低频更新的资源（`js/css`）变动了，只用在高频变动的资源文件（`html`）里做入口的改动。

## Cookies

`HTTP Cookie`（也叫 `Web Cookie` 或浏览器 `Cookie`）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。

### 创建 cookie

`Set-Cookie` 响应头部和 `Cookie` 请求头部

```http
Set-Cookie: <cookie名>=<cookie值>
```

### 会话期Cookie

会话期Cookie是最简单的 `Cookie`：浏览器关闭之后它会被自动删除，也就是说它仅在会话期内有效。会话期 `Cookie` 不需要指定过期时间（`Expires`）或者有效期（`Max-Age`）。需要注意的是，有些浏览器提供了会话恢复功能，这种情况下即使关闭了浏览器，会话期 `Cookie` 也会被保留下来，就好像浏览器从来没有关闭一样

### 持久性Cookie

和关闭浏览器便失效的会话期 `Cookie` 不同，持久性 `Cookie` 可以指定一个特定的过期时间（`Expires`）或有效期（`Max-Age`）。

```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
 
```

### Cookie的Secure和HttpOnly 标记

标记为 `Secure` 的 `Cookie` 只应通过被 `HTTPS` 协议加密过的请求发送给服务端。

标记为 `Secure` 的 `Cookie` 只应通过被 `HTTPS` 协议加密过的请求发送给服务端。但即便设置了 `Secure`  标记，敏感信息也不应该通过 `Cookie` 传输，因为 `Cookie` 有其固有的不安全性，`Secure` 标记也无法提供确实的安全保障

通过 `JavaScript` 的 `Document.cookie` `API`  是无法访问带有 `HttpOnly` 标记的 `cookie`。这么做是为了避免跨域脚本攻击（`XSS`）

```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

### Cookie的作用域

`Domain` 和 `Path` 标识定义了 `Cookie` 的作用域：即 `Cookie` 应该发送给哪些 `URL`。

`Domain` 标识指定了哪些主机可以接受 `Cookie`。如果不指定，默认为当前的主机（不包含子域名）。如果指定了 `Domain`，则一般包含子域名。

例如，如果设置 `Domain=mozilla.org`，则 `Cookie` 也包含在子域名中（如`developer.mozilla.org`）。

`Path` 标识指定了主机下的哪些路径可以接受 `Cookie`（该 `URL` 路径必须存在于请求 `URL` 中）。以字符 %x2F ("/") 作为路径分隔符，子路径也会被匹配。

例如，设置 `Path=/docs`，则以下地址都会匹配：

```js
/docs
/docs/Web/
/docs/Web/HTTP
```

### SameSite Cookies

`SameSite Cookie` 允许服务器要求某个 `cookie` 在跨站请求时不会被发送，从而可以阻止跨站请求伪造攻击

- `None` 浏览器会在同站请求、跨站请求下继续发送 `cookies`，不区分大小写。【旧版本 `chrome` 默认 `Chrome 80` 版本之前】
- `Strict` 浏览器将只在访问相同站点时发送 `cookie`。
- `Lax` 将会为一些跨站子请求保留，如图片加载或者 `frames` 的调用，但只有当用户从外部站点导航到 `URL` 时才会发送。如 `link` 链接

```
Set-Cookie: key=value; SameSite=Strict
 
None Strict Lax
```

在新版本的浏览器（`Chrome 80` 之后）中，`SameSite` 的默认属性是 `SameSite=Lax`。换句话说，当 `Cookie` 没有设置 `SameSite` 属性时，将会视作 `SameSite` 属性被设置为 `Lax` —— 这意味着 `Cookies` 将不会在当前用户使用时被自动发送。如果想要指定 `Cookies` 在同站、跨站请求都被发送，那么需要明确指定`SameSite` 为 `None`。因为这一点，我们需要好好排查旧系统是否明确指定 `SameSite`，以及推荐新系统明确指定 `SameSite`，以兼容新旧版本 `Chrome`

更多 `cookie` 相关，可以查看我之前总结的一篇关于 `cookie` 的文章 [前端须知的 Cookie 知识小结](https://juejin.im/post/6844903841909964813)

## HTTP访问控制（CORS）

跨域资源共享（`CORS`）是一种机制，它使用额外的 `HTTP` 头告诉浏览器，让运行在一个 `origin` (`domain`) 上的 `web`  应用被准许访问来自不同源服务器上的指定的资源



![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/35e96a12bc10448da41d024709d36139~tplv-k3u1fbpfcp-zoom-1.image)



跨域资源共享标准新增了一组 `HTTP` 首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源。

### 简单请求

简单请求（不会触发 `CORS` 的预检请求）需要同时满足以下三点：

- 方法是 `GET/HEAD/POST` 之一
- `Content-Type` 的值仅限 `text/plain`、`multipart/form-data`、`application/x-www-form-urlencoded` 三者之一
- `HTTP` 头部不能超过以下字段：`Accept`、`Accept-Language`、`Content-Language` `Content-Type`（需要注意额外的限制）`DPR`、`Downlink`、`Save-Data`、`Viewport-Width`、`Width`

以下为一个简单请求的请求报文以及响应报文



![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f02fa28056bc40cc8e4d7e855dec5190~tplv-k3u1fbpfcp-zoom-1.image)



简化以下：



![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/563e2b933d8a4b2aa4413dd9c00751ef~tplv-k3u1fbpfcp-zoom-1.image)



请求首部字段 `Origin` 表明该请求来源于 `http://foo.example`

本例中，服务端返回的 `Access-Control-Allow-Origin: *` 表明，该资源可以被任意外域访问。如果服务端仅允许来自 `http://foo.example` 的访问，该首部字段的内容如下：

```http
Access-Control-Allow-Origin: http://foo.example
```

`Access-Control-Allow-Origin` 应当为 * 或者包含由 `Origin` 首部字段所指明的域名。

### 预检请求

规范要求，对那些可能对服务器数据产生副作用的 `HTTP` 请求方法。浏览器必须首先使用 `OPTIONS` 方法发起一个预检请求（`preflight request`），从而获知服务端是否允许该跨域请求。

服务器确认允许之后，才发起实际的 `HTTP` 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 `Cookies` 和 `HTTP` 认证相关数据）



![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7d36ba491b2a470fa74afa9be1b649c7~tplv-k3u1fbpfcp-zoom-1.image)



预检请求中同时携带了下面两个首部字段：

```http
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type
 
```

首部字段 `Access-Control-Request-Method` 告知服务器，实际请求将使用 `POST` 方法。首部字段 `Access-Control-Request-Headers` 告知服务器，实际请求将携带两个自定义请求首部字段：`X-PINGOTHER` 与 `Content-Type`。服务器据此决定，该实际请求是否被允许。

预检请求的响应中，包括了以下几个字段

```http
Access-Control-Allow-Origin: http://foo.example
// 表明服务器允许客户端使用 POST, GET 和 OPTIONS 方法发起请求
Access-Control-Allow-Methods: POST, GET, OPTIONS
// 表明服务器允许请求中携带字段 X-PINGOTHER 与 Content-Type
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
// 表明该响应的有效时间为 86400 秒，也就是 24 小时。在有效时间内，浏览器无须为同一请求再次发起预检请求。
Access-Control-Max-Age: 86400
```

一般而言，对于跨域 `XMLHttpRequest` 或 `Fetch` 请求，浏览器不会发送身份凭证信息。如果要发送凭证信息，需要设置 `XMLHttpRequest` 的某个特殊标志位。比如说 `XMLHttpRequest` 的 `withCredentials` 标志设置为 `true`，则可以发送 `cookie` 到服务端。

对于附带身份凭证的请求，服务器不得设置 `Access-Control-Allow-Origin` 的值为“*”。 这是因为请求的首部中携带了 `Cookie` 信息，如果 `Access-Control-Allow-Origin` 的值为“*”，请求将会失败。而将 `Access-Control-Allow-Origin` 的值设置为 `http://foo.example`，则请求将成功执行。

`CORS` 涉及到的请求和响应头如下： `HTTP` 响应首部字段

- `Access-Control-Allow-Origin` 允许访问该资源的外域 `URI`。对于不需要携带身份凭证的请求，服务器可以指定该字段的值为通配符，表示允许来自所有域的请求。
- `Access-Control-Expose-Headers` 头让服务器把允许浏览器访问的头放入白名单
- `Access-Control-Max-Age` 头指定了 `preflight` 请求的结果能够被缓存多久
- `Access-Control-Allow-Credentials` 头指定了当浏览器的 `credentials` 设置为 `true` 时是否允许浏览器读取 `response` 的内容。
- `Access-Control-Allow-Methods` 首部字段用于预检请求的响应。其指明了实际请求所允许使用的 `HTTP` 方法。
- `Access-Control-Allow-Headers` 首部字段用于预检请求的响应。其指明了实际请求中允许携带的首部字段。

`HTTP` 请求首部字段

- `Origin` 首部字段表明预检请求或实际请求的源站
- `Access-Control-Request-Method` 首部字段用于预检请求。其作用是，将实际请求所使用的 HTTP 方法告诉服务器。
- `Access-Control-Request-Headers` 首部字段用于预检请求。其作用是，将实际请求所携带的首部字段告诉服务器。

