##### get 和 post 的区别？有那些请求方式？
- 请求方式：
GET POST HEAD OPTIONS PUT DELETE TRACE CONNECT
- 区别：
先引⼊副作⽤和幂等的概念。
副作⽤指对服务器上的资源做改变，搜索是⽆副作⽤的，注册是副作⽤的。
幂等表示执行相同的操作，结果也是相同的，⽐如注册10 个和 11 个帐号是不幂等的，对⽂章进⾏获取 10 次和 11 次是幂等的。
  - 在规范的应⽤场景上说：Get 多⽤于⽆副作⽤，幂等的场景，例如搜索关键字。Post 多⽤于
副作⽤，不幂等的场景，例如注册
  - 在技术上说：
    - Get 请求能缓存，Post 不能 
    - Post 相对 Get 安全⼀点点，因为Get 请求都包含在 URL ⾥，且会被浏览器保存历史纪录，Post 不会，但是在抓包的情况下都是⼀样的。
    - Post 可以通过 request body来传输⽐ Get 更多的数据，Get 没有这个技术
    - URL有⻓度限制，会影响 Get 请求，但是这个⻓度限制是浏览器规定的，不是技术规定的
    - Post ⽀持更多的编码类型且不对数据类型限制,GET 只能进行 URL 编码，只能接收 ASCII 字符
    - 从TCP的角度，GET 请求会把请求报文一次性发出去，而 POST 会分为两个 TCP 数据包，首先发 header 部分，如果服务器响应 100(continue)， 然后发 body 部分。(火狐浏览器除外，它的 POST 请求只发一个 TCP 包)


##### 知道哪些请求方式？为什么要分这么多的请求方式？
get post head put delete options trace connect


##### patch 和 put 的区别？
PATCH：更新部分资源，非幂等，副作用
PUT：更新整个资源，具有幂等性，副作用