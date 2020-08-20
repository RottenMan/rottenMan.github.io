---
categories:
    - HTTP网络协议
tags:
    - HTTP网络协议
date: 2020-08-20
---



## Cookie

### Cookie是什么

Cookie 是服务器发送到用户浏览器并保存在本地的一小块数据，会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。

![](https://s1.ax1x.com/2020/08/20/dGZ4ne.png)



### Cookie的作用域

Domain 和 Path 标识定义了 Cookie 的作用域，即 Cookie 应该发送给哪些 URL.

- Domain 标识指定了哪些主机可以接受 Cookie
  - 如果不指定，默认为当前文档的主机（不包含子域名）。
  - 如果指定了 Domain，则一般包含子域名。例如，如果设置 Domain=`wolongxueyuan.com`, 则（Cookie 也包含在子域名中（如 `developer.wolongxueyuan.com`).
- Path 标识指定了主机下的哪些路径可以接受 Cookie（该 URL 路径必须存在于请求 URL 中）。以字符 % x2F (“/”）作为路径分隔符，子路径也会被匹配。



### Cookie的有效期

Max-Age 和 Expires / 标识定义了 Cookie 的有效期，即 Cookie 的生命周期。

- 会话期 Cookie

  会话期 Cookie 是最简单的 Cookie。浏览器关之后 Cookie 会被自动删除，也就是说 cookie 仅在会话期内有效。会话期 Cookie 不需要指定过期时间（Expires）或者有效期（Max ー Age)

- 持久性 Cookie

  持久性 Cookie 可以指定一个特定的过期时间（ Expires）或有效期（Max-Age)



### Cookie的应用

**Cookie 主要用于以下三个方面**

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）



## 访问与更新Cookie

### 创建Cookie

**创建Cookie的语法👇**

```
document.cookie = newCookie
```

* newCookie是一个键值对形式的字符串
* 这个方法一次只能对一个cookie进行设置或更新



### Cookie的存储的数据格式

* 多个数据内容之间使用 ";" 分隔
* 每个数据内容都是key/value格式
* 允许存储多个数据内容

```javascript
document.cookie = 'name=wangergou;Domain=www.baidu.com';
```



### 删除Cookie

* 将对应的值设置为空
* 如果是持久性的Cookie，将时间设置为一个过期时间Cookie就会失效
* 删除时不必指定Cookie的值

```javascript
document.cookie = 'name=;expires=Thu,01 jan 1970 00:00:00 GMT'
```



## HTTP中的Cookie

### Set-Cookie 响应头

服务器使用 Set-Cookie 响应头部向用户代理（一般是浏览器）发送 cookie 信息

```javascript
Set-Cookie: <cookie名>=<cookie值>
```



**服务器通过该头部告知客户端保存Cookie信息**

```javascript
HTTP/1.0 200 ok
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry
```



### Cookie请求头

对服务器发起的每一次新请求，浏览器都会将之前保存的 Cookie 信息通过 Cookie 请求头部再发送给服务器。

```javascript
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco;tasty_cookie=strawberry
```

