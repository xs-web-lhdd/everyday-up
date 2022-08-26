##### 一文搞懂 Promise async await：

https://juejin.cn/post/6844903607968481287

#### Promise 追尾？
[三元大佬](https://juejin.cn/post/6844904004007247880#heading-36)

##### 8.3 async/await 和 Promise 的关系

- async/await 是消灭异步回调的终极武器。
- 但和 Promise 并不互斥，反而，两者相辅相成。
- 执行 async 函数，返回的一定是 Promise 对象。
- await 相当于 Promise 的 then。
- tru...catch 可捕获异常，代替了 Promise 的 catch。