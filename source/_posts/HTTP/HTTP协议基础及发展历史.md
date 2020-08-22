---
categories:
    - HTTP网络协议
tags:
    - HTTP网络协议
date: 2020-08-22
---



## 5层网络模型介绍

![](https://s1.ax1x.com/2020/08/22/ddJajK.png)

### 第三层

* 物理层

  **主要作用是定义物理设备如何传输数据**

* 数据链路层

  **在通信的实体间建立数据链路连接**

* 网络层

  **数据在节点之间传输创建逻辑链路**



### 传输层

向用户提供可靠的端到端（End-to-End）服务

传输层向高层屏蔽了下层数据通信的细节



### 应用层

* 为应用软件提供了很多服务
* 构建与TCP协议之上
* 屏蔽网络传输相关细节

## HTTP协议的发展历史

### HTTP/0.9

- 只有一个命令 GET
- 没有 HEADER 等描述数据的信息
- 服务器发送完毕，就关闭 TCP 连接

### HTTP/1.0

- 增加了很多命令
- 增加 status code 和 header
- 多字符集支持、多部分发送、权限、缓存等

### HTTP/1.1

- 持久连接
- pipeline
- 增加了 host 和其他一些命令

### HTTP2

- 所有数据以二进制传输
- 同一个连接里面发送多个请求不在需要按照顺序来
- 头信息压缩以及推送提高效率的功能

## HTTP的三次握手

![](https://s1.ax1x.com/2020/08/22/ddJUc6.png)

举个栗子：把客户端比作男孩，服务器比作女孩。用他们的交往来说明“三次握手”过程：

（1）男孩喜欢女孩，于是写了一封信告诉女孩：我爱你，请和我交往吧！;写完信之后，男孩焦急地等待，因为不知道信能否顺利传达给女孩。

（2）女孩收到男孩的情书后，心花怒放，原来我们是两情相悦呀！于是给男孩写了一封回信：我收到你的情书了，也明白了你的心意，其实，我也喜欢你！我愿意和你交往！;

写完信之后，女孩也焦急地等待，因为不知道回信能否能顺利传达给男孩。

（3）男孩收到回信之后很开心，因为发出的情书女孩收到了，并且从回信中知道了女孩喜欢自己，并且愿意和自己交往。然后男孩又写了一封信告诉女孩：你的心意和信我都收到了，谢谢你，还有我爱你！

女孩收到男孩的回信之后，也很开心，因为发出的情书男孩收到了。由此男孩女孩双方都知道了彼此的心意，之后就快乐地交流起来了~~

这就是通俗版的“三次握手”，期间一共往来了三封信也就是“三次握手”，以此确认两个方向上的数据传输通道是否正常



> **为什么要进行第三次握手？**
>
> 为了防止服务器端开启一些无用的连接增加服务器开销以及防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误

## URI-URL和URN

### URI

Uniform Resource Identifier ：统一资源标识符

用来唯一标识互联网上的信息资源，包括 URL 和 URN

### URL

Uniform Resource Locator：统一资源定位器

### URN

永久统一资源定位符，资源移动之后还能被找到。但目前还没有非常成熟的使用方案

## HTTP报文格式

![](https://s1.ax1x.com/2020/08/22/ddJN1x.png)

### HTTP 方法

用来定义对于资源的操作，常用的操作有 GET、POST 等。

### HTTP CODE

定义服务器对请求的处理结果，各个区间的 CODE 有各自的语义。好的 HTTP 服务可以通过 CODE 判断结果。

## 实现一个最简单的web服务器

```javascript
const http = require('http')

const port = 3000

const server = http.createServer((req, res) => {
  res.statusCode = 200
  res.setHeader('Content-Type', 'text/plain')
  res.end('你好世界\n')
})

server.listen(port, () => {
  console.log(`服务器运行在 http://${hostname}:${port}/`)
})
```

简要分析一下。 这里引入了 `http` 模块

使用该模块来创建 HTTP 服务器。

服务器被设置为在指定的 `3000` 端口上进行监听。 当服务器就绪时，则 `listen` 回调函数会被调用。

传入的回调函数会在每次接收到请求时被执行。 每当接收到新的请求时，`request` 事件会被调用，并提供两个对象：一个请求（[`http.IncomingMessage`](http://nodejs.cn/api/http.html#http_class_http_incomingmessage) 对象）和一个响应（[`http.ServerResponse`](http://nodejs.cn/api/http.html#http_class_http_serverresponse) 对象）。

`request` 提供了请求的详细信息。 通过它可以访问请求头和请求的数据。

`response` 用于构造要返回给客户端的数据。