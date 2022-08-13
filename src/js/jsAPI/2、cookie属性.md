##### 讲讲 cookie 的属性：
[菜鸟教程---无除了 path 之外的属性讲解](https://www.runoob.com/js/js-cookies.html)
[cookie详细属性讲解](https://blog.csdn.net/qq_39834073/article/details/107808959)

Domain
Domain决定Cookie在哪个域是有效的，也就是决定在向该域发送请求时是否携带此Cookie，Domain的设置是对子域生效的，如Doamin设置为 .a.com,则b.a.com和c.a.com均可使用该Cookie，但如果设置为b.a.com,则c.a.com不可使用该Cookie。Domain参数必须以点(".")开始。
 

Path
Path是Cookie的有效路径，和Domain类似，也对子路径生效，如Cookie1和Cookie2的Domain均为a.com，但Path不同，Cookie1的Path为 /b/,而Cookie的Path为 /b/c/,则在a.com/b页面时只可以访问Cookie1，在a.com/b/c页面时，可访问Cookie1和Cookie2。Path属性需要使用符号“/”结尾。
 

Expires/Max-age
Expires和Max-age均为Cookie的有效期，Expires是该Cookie被删除时的时间戳，格式为GMT,若设置为以前的时间，则该Cookie立刻被删除，并且该时间戳是 服务器 时间（绝对时间），不是本地时间！若不设置则默认页面关闭时删除该Cookie。
Max-age也是Cookie的有效期，但它的单位为秒（相对时间），即多少秒之后失效，若Max-age设置为0，则立刻失效，设置为负数，则在页面关闭时失效。Max-age默认为 -1。
 

Size
Szie是此Cookie的大小。在所有浏览器中，任何cookie大小超过限制都被忽略，且永远不会被设置。一般不会超过 4kb

HttpOnly
HttpOnly值为 true 或 false,若设置为true，则不允许通过脚本document.cookie去更改这个值，同样这个值在document.cookie中也不可见，但在发送请求时依旧会携带此Cookie。
 

Secure
Secure为Cookie的安全属性，若设置为true，则浏览器只会在HTTPS和SSL等安全协议中传输此Cookie，不会在不安全的HTTP协议中传输此Cookie。
 

SameSite
SameSite用来限制第三方 Cookie，从而减少安全风险。它有3个属性，分别是：

- Strict：最为严格，完全禁止第三方Cookie，跨站点时，任何情况下都不会发送Cookie

- Lax：规则稍稍放宽，大多数情况也是不发送第三方 Cookie，但是导航到目标网址的 Get 请求除外。

- None：网站可以选择显式关闭SameSite属性，将其设为None。不过，前提是必须同时设置Secure属性（Cookie 只能通过 HTTPS 协议发送），否则无效。