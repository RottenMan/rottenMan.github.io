---
categories:
    - HTTP网络协议
tags:
	 - 图解HTTP
date: 2020-8-18
---



## HTTP 协议用于客户端和服务器端之间的通信
请求访问文本或图像等资源的一端称为客户端， 而提供资源响应的一端称为服务器端

 HTTP 协议能够明确区分哪端是客户端， 哪端是服务器端

![](https://s1.ax1x.com/2020/08/19/d1zwQ0.png)



## 通过请求和响应的交换达成通信

**HTTP 协议规定， 请求从客户端发出， 最后服务器端响应该请求并返回**



请求报文内容

```
GET /index.htm HTTP/1.1
Host: hackr.jp
```

* 开头的GET表示请求访问服务器的类型， 称为方法
* 随后的字符串 /index.htm 指明了请求访问的资源对象，也叫做请求 URI
* 最后的 HTTP/1.1， 即 HTTP 的版本号
* 请求报文是由请求方法、 请求 URI、 协议版本、 可选的请求首部字段和内容实体构成的



响应报文内容

```
HTTP/1.1 200 OK
Date: Tue, 10 Jul 2012 06:50:15 GMT
Content-Length: 362
Content-Type: text/html
```

*  HTTP/1.1 表示服务器对应的 HTTP 版本
* 200 OK 表示请求的处理结果的状态码和原因短语
* Date显示了创建响应的日期时间
* 之后的内容称为资源实体的主体
* 响应报文基本上由协议版本、 状态码用以解释状态码的原因短语、 可选的响应首部字段以及实体主体构成



## HTTP是不保存状态的协议

* HTTP 是一种不保存状态， 即无状态（stateless） 协议
* 为了实现保持状态功能，HTTP协议有了Cookie



## 请求URI定位资源

![](https://s1.ax1x.com/2020/08/20/dJQkQI.png)

* HTTP 协议使用 URI 定位互联网上的资源

* 如不是访问特定资源而是对服务器本身发起请求， 可以用一个 * 来代替请求 URI

  ```text
  OPTIONS * HTTP/1.1
  ```

  

## 告知服务器意图的HTTP方法

### GET：获取资源

**GET 方法用来请求访问已被 URI 识别的资源**

| 请求 | GET /index.html HTTP/1.1<br />Host: www.hackr.jp |
| ---- | ------------------------------------------------ |
| 响应 | 返回 index.html 的页面资源                       |



### POST：传输实体主体

**POST 方法用来传输实体的主体。**

| 请求 | POST /submit.cgi HTTP/1.1<br />Host: www.hackr.jp<br />Content-Length: 1560（1560字节的数据） |
| ---- | ------------------------------------------------------------ |
| 响应 | 返回 submit.cgi 接收数据的处理结果                           |



### PUT：传输文件

* PUT 方法用来传输文件
* PUT方法存在安全性问题， 因此一般的 Web 网站不使用该方法

| 请求 | PUT /example.html HTTP/1.1<br/>Host: www.hackr.jp<br/>Content-Type: text/html<br/>Content-Length: 1560（1560 字节的数据） |
| ---- | ------------------------------------------------------------ |
| 响应 | 响应返回状态码 204 No Content（比如 ： 该 html 已存在于服务器上） |



### HEAD：获得报文首部

**HEAD 方法和 GET 方法一样， 只是不返回报文主体部分。 用于确认URI 的有效性及资源更新的日期时间等**

| 请求 | HEAD /index.html HTTP/1.1<br/>Host: www.hackr.jp |
| ---- | ------------------------------------------------ |
| 响应 | 返回index.html有关的响应首部                     |



### OPTIONS：询问支持的方法

**OPTIONS 方法用来查询针对请求 URI 指定的资源支持的方法**

| 请求 | OPTIONS * HTTP/1.1<br/>Host: www.hackr.jp                    |
| ---- | ------------------------------------------------------------ |
| 响应 | HTTP/1.1 200 OK<br/>Allow: GET, POST, HEAD, OPTIONS<br/>（返回服务器支持的方法） |

![](https://s1.ax1x.com/2020/08/20/dJQmTS.png)



## 使用方法下达命令

向请求 URI 指定的资源发送请求报文时， 采用称为方法的命令。方法中有 GET、 POST 和 HEAD 等

![](https://s1.ax1x.com/2020/08/20/dJQKYQ.png)

**HTTP/1.0 和 HTTP/1.1 支持的方法（方法名区分大小写， 注意要用大写字母）👇**

| 方法    | 说明                   | 支持的 HTTP 协议版本 |
| ------- | ---------------------- | -------------------- |
| GET     | 获取资源               | 1.0  1.1             |
| POST    | 传输实体主体           | 1.0  1.1             |
| PUT     | 传输文件               | 1.0  1.1             |
| HEAD    | 获得报文首部           | 1.0  1.1             |
| DELETE  | 删除文件               | 1.0  1.1             |
| OPTIONS | 询问支持的方法         | 1.1                  |
| TRACE   | 追踪路径               | 1.1                  |
| CONNECT | 要求用隧道协议连接代理 | 1.1                  |
| LINK    | 建立和资源之间的联系   | 1.0                  |
| UNLINE  | 断开连接关系           | 1.0                  |



## 持久连接节省通信量

### 持久连接

*  持久连接的特点是， 只要任意一端没有明确提出断开连接， 则保持 TCP 连接状态
* 持久连接的好处在于减少了 TCP 连接的重复建立和断开所造成的额外开销， 减轻了服务器端的负载
* 在 HTTP/1.1 中， 所有的连接默认都是持久连接

![](https://s1.ax1x.com/2020/08/20/dJQMWj.png)

### 管线化

 从前发送请求后需等待并收到响应， 才能发送下一个请求，管线化技术可以不用等待响应亦可直接发送下一个请求，这样就能够做到同时并行发送多个请求， 而不需要一个接一个地等待响应

![](https://s1.ax1x.com/2020/08/20/dJQuFg.png)

##  使用 Cookie 的状态管理

* Cookie技术通过在请求喝响应报文中写入Cookie信息来控制客户端的状态

* Cookie会根据从服务器端发送的响应报文内的一个叫做Set-Cookie的首部字段信息，通知客户端保存Cookie。

* 当下一次客户端再往该服务器发送请求时，客户端会自动在请求报文中加入Cookie值后发送出去



请求报文（ 没有 Cookie 信息的状态）

```text
GET /reader/ HTTP/1.1
Host: hackr.jp
*首部字段内没有Cookie的相关信息
```

响应报文（ 服务器端生成 Cookie 信息）

```text
HTTP/1.1 200 OK
Date: Thu, 12 Jul 2012 07:12:20 GMT
Server: Apache
＜Set-Cookie: sid=1342077140226724; path=/; expires=Wed,
10-Oct-12 07:12:20 GMT＞
Content-Type: text/plain; charset=UTF-8
```

请求报文（ 自动发送保存着的 Cookie 信息）

```text
GET /image/ HTTP/1.1
Host: hackr.jp
Cookie: sid=1342077140226724
```

