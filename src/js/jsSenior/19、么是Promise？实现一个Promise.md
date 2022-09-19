##### 一文搞懂 Promise async await：

https://juejin.cn/post/6844903607968481287

#### Promise 追尾？
[三元大佬](https://juejin.cn/post/6844904004007247880#heading-36)

#### Promise.all 参数数组中如果一个出错了，那么其他还会执行吗？
error不影响其他任务，只是最后返回的promise状态为rejected

#### 在 `new Promise` 时传入的执行函数里面 `throw Error` 会怎么样？
1、如果没有 catch 会报：` UnhandledPromiseRejectionWarning: Error: xxxxxx（throw 出来的错误信息）`
2、如果有 catch 会被捕获到

##### 8.3 async/await 和 Promise 的关系

- async/await 是消灭异步回调的终极武器。
- 但和 Promise 并不互斥，反而，两者相辅相成。
- 执行 async 函数，返回的一定是 Promise 对象。
- await 相当于 Promise 的 then。
- tru...catch 可捕获异常，代替了 Promise 的 catch。


### ES6 实现的 Promise 和 Promise 规范有啥区别？是否兼容？怎么查看网站是否兼容？


### 如果浏览器不兼容 Promise，如何解决？
可以参考 Babel 将 ES6 转化为 ES5，怎么做的？

### 如何查找解决方案，用的什么搜索？