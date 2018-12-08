# HTML5

## HTML 语义化

- 语义化的含义就是用正确的标签做正确的事情，html语义化就是让页面的内容结构化，便于对浏览器、搜索引擎解析；

## localStorage、sessionStorage、cookies 的区别

`localStorage` 本地存储，长期存在，大小一般为 5Mb，同源策略，因此可以用来跨页面交互数据

`sessionStorage` 同上，但关闭浏览器就会消失

- 常用的一些属性，方法：

```JavaScript
Storage.length // 返回一个整数，数据项数量
Storage.setItem(key, value) // 储存 / 更新
Storage.getItem(key) // 读取
Storage.removeItem(key) // 删除
Storage.clear() // 清空
```

`cookies` 服务器发送到用户浏览器，并且保存在浏览器上的一块数据，会在下次发送请求时携带，并发送给服务器。

- 有两种 `cookies`, `session cookies` 和 `persistent cookies`, `persistent cookies` 可以设置过期时间 `Expires` 或者有效期 `Max-Age`
- `session cookies` 和 `persistent cookies` 的[对比](https://www.cisco.com/c/en/us/support/docs/security/web-security-appliance/117925-technote-csc-00.html)写的很清楚：

```plaintext
Types of Cookies
There are two different types of cookies - session cookies and persistent cookies. If a cookie does not contain an expiration date, it is considered a session cookie. Session cookies are stored in memory and never written to disk. When the browser closes, the cookie is permanently lost from this point on. If the cookie contains an expiration date, it is considered a persistent cookie. On the date specified in the expiration, the cookie will be removed from the disk.
```
- 可以设置作用域 `domain`, 路径 `path`
- `secure` 标记 `https` 协议传输
- 大小为 4kb
- 使用方法：

```JavaScript
    document.cookie = "yummy_cookie=choco"
    document.cookie = "tasty_cookie=strawberry"
    console.log(document.cookie)
```
