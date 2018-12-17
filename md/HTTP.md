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

# 跨域（jsonp, postMessage, cors, window.name, 用服务器(比如 node)转发请求和响应）

## 跨域有哪些常见的解决方式
  - `cors` 设置被跨域服务器，可以接受来自 a.com 的请求（其实是允许某个地址可以访问）
  - `jsonp` ，用 script src 去引用，获取信息
  - 架设 `node` 本地转发请求，或者请求后端转发请求 （这个不是很了解）

## window.name

在客户端浏览器中每个页面都有一个独立的窗口对象 `window`，默认情况下`window.name`为空，在窗口的生命周期中，载入的所有页面共享一个 `window.name` 并且每个页面都有对此读写的权限，`window.name` 会一直存在当前窗口，但存储的`字符串`不超过`2M`。

例子：

```js
> window.name
""

> window.name='test'
"test"
// 在控制台输入, 不要直接打开网址
> location.href='http://www.bilibili.com'
"http://www.bilibili.com"
Navigated to https://www.bilibili.com/

> window.name
"test"
```

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

`csrf` 是跨站攻击，拿到了用户的信息之后，伪造用户请求去访问接口

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

Body
```

Body部分略过
其中响应行（第一行）：
- HTTP/1.1 是版本
- 301 是「状态码」
- Moved Permanently 是状态码的描述

浏览器会自己解析Header部分，然后将Body显示成网页

## 从输入 `URL` 到页面加载完成的过程

[参考链接](https://juejin.im/post/5bf3ad55f265da61682afc9b)

总体来说分为以下几个过程:

1. `DNS` 解析:将域名解析成 `IP` 地址
2. `TCP` 连接：`TCP` 三次握手
3. 发送 `HTTP` 请求
4. 服务器处理请求并返回 `HTTP` 报文
5. 浏览器解析渲染页面
6. 断开连接：`TCP` 四次挥手

## Ajax 过程：
Ajax: 浏览器提供的使用 `HTTP` 协议收发数据的接口

```
0 1 2 3 4 各代表什么含义

0 代理被创建，但并未 open()
1 已经调用 open()
2 已经调用 send() 并且头部和状态已经获得了
3 正在下载
4 下载完成
```
```JavaScript
const ajax = (method, path, data, responseCallback) => {
    // 发送登录数据
    let r = new XMLHttpRequest()
    // 设置请求方法和请求地址
    r.open(method, path, true)
    // 设置发送的数据的格式
    r.setRequestHeader('Content-Type', 'application/json')
    // 注册响应函数
    r.onreadystatechange = () => {
        if(r.readyState == 4) {
            const response = JSON.parse(r.response)
            responseCallback(response)
        }
    }
    r.send(data)
}
```

## 事件冒泡, 事件委托

事件触发有三个阶段

- `window` 往事件触发处传播，遇到注册的捕获事件会触发
- 传播到事件触发处时触发注册的事件
- 从事件触发处往 `window` 传播，遇到注册的冒泡事件会触发

事件触发一般来说会按照上面的顺序进行，但是也有特例，如果给一个目标节点同时注册冒泡和捕获事件，事件触发会按照注册的顺序执行。

```js
// 以下会先打印冒泡然后是捕获
node.addEventListener('click', event =>{
    console.log('冒泡')
}, false)

node.addEventListener('click', event =>{
    console.log('捕获 ')
}, true)
```

一般来说，我们只希望事件只触发在目标上，这时候可以使用 `stopPropagation` 来阻止事件的进一步传播。通常我们认为 `stopPropagation` 是用来阻止事件冒泡的，其实该函数也可以阻止捕获事件。`stopImmediatePropagation` 同样也能实现阻止事件，但是还能阻止该事件目标执行别的注册事件。

```js
node.addEventListener('click', event =>{
    event.stopImmediatePropagation()
    console.log('冒泡')
}, false)
// 点击 node 只会执行上面的函数，该函数不会执行
node.addEventListener('click', event => {
    console.log('捕获 ')
}, true)
```
## 事件委托

如果一个节点中的子节点是动态生成的，那么子节点需要注册事件的话应该注册在父节点上

```html
<ul id="ul">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
<script>
    let ul = document.querySelector('#ul')
    ul.addEventListener('click', event => {
        console.log(event.target)
    })
</script>
```

事件代理的方式相对于直接给目标注册事件来说，有以下优点:

- 节省内存
- 不需要给子节点注销事件

## 性能优化

### 优化原则和方向

性能优化的原则是以更好的用户体验为标准，具体就是实现下面的目标：

1. 多使用内存、缓存或者其他方法
2. 减少 `CPU` 和 `GPU` 计算，更快展现

优化的方向有两个：
- 减少页面体积，提升网络加载
- 优化页面渲染

### 减少页面体积，提升网络加载
- 静态资源的压缩合并（`JS` 代码压缩合并、`CSS` 代码压缩合并、雪碧图）
- 静态资源缓存（资源名称加 MD5 戳）
- 使用 `CDN` 让资源加载更快

### 优化页面渲染
- `CSS` 放前面，`JS` 放后面
- 懒加载（图片懒加载、下拉加载更多）
- 减少 `DOM` 查询，对 `DOM` 查询做缓存
- 减少 `DOM` 操作，多个操作尽量合并在一起执行（DocumentFragment）
- 事件节流
- 尽早执行操作（DOMContentLoaded）
- 使用 `SSR` 后端渲染，数据直接输出到 `HTML` 中，减少浏览器使用 `JS` 模板渲染页面 `HTML` 的时间
