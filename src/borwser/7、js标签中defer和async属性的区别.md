### 7、js 标签中 defer 和 async 属性的区别？
- async 是异步执行，异步下载完毕后就会执行，`不确保执行顺序`，一定在 onload 前，但`不确定在 DOMContentLoaded 事件的前或后`
- defer 是延迟执行，在浏览器看起来的效果像是将`脚本放在了 body 后面`一样（虽然按`规范应该是在 DOMContentLoaded 事件前`，但实际上不同浏览器的优化效果不一样，也有可能在它后面）