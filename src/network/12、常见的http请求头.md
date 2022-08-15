##### 12、常见的http请求头：
常见的请求头字段：
- Host: 服务端的域名
- Accept、Accept-Encoding、Accept-Language：与服务端的协商字段
- Content-Type：浏览器请求体内容的类型
- Content-Length：实体数据的字节长度。
- Referer：请求的页面来源。
- User-Agent：用户代理，一些厂商、设备、版本等信息。
- Cookie：用户的Cookie信息。
- 还有上面提到的缓存相关字段

常见的响应头字段：
- Access-Control-Allow-Origin：指定哪些网站可以跨域源资源共享。
- Access-Control-Allow-Methods：允许的http请求方法
- Content-Type：服务端响应体内容的类型。
- Content-Length：实体数据的字节长度，如果设置短了数据会丢失，设置长了会导致请求失败。
- Set-Cookie：与请求头中的 Cookie 对应
- 还有上面提到的缓存相关字段

**请求头里的Referer是干什么用的？**
设置一些防盗链，比如直接在浏览器的地址栏中输入一个资源的URL地址，那么这种请求是不会包含Referer的。

**Content-Length是什么的长度？**
实体的传输字节长度。（实体长度和实体的传输长度是有区别，比如说gzip压缩下，消息实体长度是压缩前的长度，消息实体的传输长度是gzip压缩后的长度）

**知道Transfer-Encoding: chunked这个字段吗？**
在数据内容不能确定，分块传输场景下使用。（无法在请求或者响应前明确指定Content-Length，所以Content-Length字段会被忽略不被发送）

**Cookie中有哪些属性？**
- Name：Cookie的名称。
- Value： Cookie的值。
- Domain： 指定Cookie的域名。
- Path：指定Cookie所属的路径。（只有匹配上Domain和Path才会附加Cookie）
- Expires/Max-Age: 过期时间
- Size：Cookie的大小
- HttpOnly：禁止通过document.cookie等方式拿到Cookie。（缓解XSS攻击）
- Secure： Cookie只能用https协议发送给服务器。
- SameSite：可以有效缓解CSRF攻击。
- SameSite=None: 浏览器会在同站请求、跨站请求下继续发送Cookie。
- SameSite=Strict: 限制Cookie不能跨站发送，只在访问相同站点时发送Cookie。
- SameSite=Lax: 跨站请求时，如果是安全的HTTP方法情况会携带Cookie。

**同源和同站的区别是什么？**
同源：scheme://ip/hostname:port一样表示同源
同站：二级域名.顶级域名一样表示同站。
[掘金好文](https://juejin.cn/post/6877496781505200142)



##### 一个请求由哪些部分构成？
请求报文和响应报文大致上都是 起始行+头部+空行+实体。

- 起始行
  请求报文起始行： 请求方法 路径 HTTP版本。
  响应报文起始行： HTTP版本 状态码 状态。

- 头部
  就是常见的那些请求头响应头字段。

- 空行
  区分头部和实体。

- 实体
  请求携带的数据或者响应返回的数据。

Q: 头部中间加一个空行会怎么样？
空行后面的内容都是实体了。
