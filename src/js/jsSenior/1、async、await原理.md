##### async await 原理？

[9k字 | Promise/async/Generator实现原理解析 - 掘金 (juejin.cn)](https://juejin.cn/post/6844904096525189128#comment)

##### async/await 与 promise 的关系：
- async/await 是消灭异步回调的终极武器。
- 但和 Promise 并不互斥，反而，两者相辅相成。
- 执行 async 函数，返回的一定是 Promise 对象。
- await 相当于 Promise 的 then。
- try...catch 可捕获异常，代替了 Promise 的 catch。