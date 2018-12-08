# HTTP

## HTTP 请求方法

- GET 获取
- POST 提交
- PUT 替换取代
- DELETE 删除
- OPTIONS 预检请求 检查是否符合post等请求的条件

## 常见状态码

- 1xx 代表请求已被接受，需要继续处理
  - 100 Continue 已接收到请求头
  - 101 服务器已理解，但需要切换协议
  - 102 Processing 服务器已经收到并在处理，但暂无相应可用
- 2xx 请求成功，服务器接收、理解、并接受
  - **200 OK 请求已成功**
  - 202 Accepted 服务器已接收，但尚未处理，也可能不会执行
  - 204 No Content 成功处理，但是没有任何返回内容
- 3xx 重定向
  - **301 被请求的资源已经永久移动到新位置**
  - **302 Found 要求客户端执行临时重定向**
  - 304 表示资源未被修改，不重新传输
  - 305 需要使用代理才可以访问
- 4xx 客户端错误
  - 400 错误请求，服务器不能或者不会处理
  - 401 未认证，用户没有必要的凭据
  - **403 访问被拒绝**
  - **404 请求失败，资源未在服务器上发现**
  - 405 请求方法不能用于获取请求的资源
  - 408 请求超时
- 5xx 服务器错误
  - 500 通用错误，但是没有错误信息
  - 501 服务器不支持
  - **502 Bad Gateway 网关或者代理的服务器执行请求时，从上游服务器接收到无效的响应**
  - **504 Gateway Timeout 网关超时**

## 头部常见字段

- Accept 期望的 MIME 类型列表
- Accept-Charset 期望的字符集
- Accept-Encoding 支持的压缩方法
- Connection
- **Content-Length 请求长度**
- **Content-Type 内容类型**
- **Host 服务器**
- Origin 访问的地址
- Referer 来自
- User-Agent
- Accept-Language 期望的页面语言
- **Request Method 请求方法**

# 跨域（jsonp, postMessage, cors, 用服务器(比如 node)转发请求和响应）

## 跨域有哪些常见的解决方式
  - `cors` 设置被跨域服务器，可以接受来自 a.com 的请求（其实是允许某个地址可以访问）
  - `jsonp` ，用 script src 去引用，获取信息
  - 架设 `node` 本地转发请求，或者请求后端转发请求 （这个不是很了解）

##  `XSS`  跨网站指令码 `Cross-site scripting`

`XSS` 通过修改 `HTML` 节点或者执行 `JavaScript` 代码来攻击网站。

如何防御?

最普遍的做法是
  - 转义输入输出的内容，对于引号，尖括号，斜杠进行转义
  - 白名单的方式。

## `CSRF` 跨站请求伪造  `Cross-site request forgery`

利用用户的登录态发起恶意请求。

防范 `CSRF` 可以遵循以下几种规则：
  - `Get` 请求不对数据进行修改
  - 不让第三方网站访问到用户 `Cookie`
  - 阻止第三方网站请求接口
  - 请求时附带验证信息，比如验证码或者 `token`

`xss` 和 `xsrf` 的原理是什么
`xss` 是 `html` 注入，用户在输入的地方插入 `script` 元素,浏览器会识别为 `JavaScript` 代码，如果这个代码有访问外部服务器，就是 `xss` 攻击
防治措施是 不信任用户的输入，将 < > 转译为 < &rt; 来处理内容
csrf 是跨站攻击，拿到了用户的信息之后，伪造用户请求去访问接口

## window.name

在客户端浏览器中每个页面都有一个独立的窗口对象 `window`，默认情况下`window.name`为空，在窗口的生命周期中，载入的所有页面共享一个 `window.name` 并且每个页面都有对此读写的权限，`window.name` 会一直存在当前窗口，但存储的`字符串`不超过`2M`。

例子：

```js
> window.name
""

> window.name='test';
"test"

> location.href='http://www.google.com';
"http://www.google.com"
Navigated to https://www.google.com/

> window.name
"test"
```

## 网址组成（四部分）

- 协议     ` http`, `https`（`https` 是加密的 `http`）
- 主机     `g.cn`  `zhihu.com`之类的网址
- 端口     ` HTTP` 协议默认是 80，因此一般不用填写
- 路径     下面的「/」和「/question/31838184」都是路径

`http://www.zhihu.com/`

`http://www.zhihu.com/question/31838184`

## 端口是什么？

一个比喻：
用邮局互相写信的时候，ip相当于地址（也可以看做邮编，地址是域名）
端口是收信人姓名（因为一个地址比如公司、家只有一个地址，但是却可能有很多收信人）
端口就是一个标记收信人的数字。
端口是一个 16 位(二进制的 位)的数字，所以范围是 0-65535（2**16）

## HTTP协议

一个传输协议，协议就是双方都遵守的规范。
为什么叫超文本传输协议呢，因为收发的是文本信息。
- 浏览器（客户端）按照规定的格式发送文本数据（请求）到服务器
- 服务器解析请求，按照规定的格式返回文本数据（响应）到浏览器
- 浏览器解析得到的数据，并做相应处理

请求和返回是一样的数据格式，分为4部分：
- 请求行或者响应行
- Header（请求的 Header 中 Host 字段是必须的，其他都是可选）
- \r\n\r\n（连续两个换行回车符，用来分隔Header和Body）
- Body（可选）

```html
GET / HTTP/1.1
Host: g.cn

Body
```

请求的格式，注意大小写（这是一个不包含Body的请求）：
原始数据如下

`'GET / HTTP/1.1\r\nhost:g.cn\r\n\r\n'`

打印出来如下

```html
GET / HTTP/1.1
Host: g.cn
```

`'GET / HTTP/1.1\r\nhost: www.qq.com\r\n\r\n'`

其中
- GET 是请求方法（还有POST等，这就是个标志字符串而已）
- / 是请求的路径（这代表根路径）
- HTTP/1.1  中，1.1是版本号，通用了20年

具体字符串是 `'GET / HTTP/1.1\r\nhost:g.cn\r\n\r\n'`
返回的数据如下

```html
HTTP/1.1 301 Moved Permanently
Alternate-Protocol: 80:quic,p=0,80:quic,p=0
Cache-Control: private, max-age=2592000
Content-Length: 218
Content-Type: text/html; charset=UTF-8
Date: Tue, 07 Jul 2015 02:57:59 GMT
Expires: Tue, 07 Jul 2015 02:57:59 GMT
Location: http://www.google.cn/
Server: gws
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
```

Body部分略过
其中响应行（第一行）：
- HTTP/1.1 是版本
- 301 是「状态码」，参见文末链接
- Moved Permanently 是状态码的描述

浏览器会自己解析Header部分，然后将Body显示成网页

## 从输入 `URL` 到页面加载完成的过程

[参考链接](https://juejin.im/post/5bf3ad55f265da61682afc9b)

总体来说分为以下几个过程:

1. DNS 解析:将域名解析成 IP 地址
2. TCP 连接：TCP 三次握手
3. 发送 HTTP 请求
4. 服务器处理请求并返回 HTTP 报文
5. 浏览器解析渲染页面
6. 断开连接：TCP 四次挥手