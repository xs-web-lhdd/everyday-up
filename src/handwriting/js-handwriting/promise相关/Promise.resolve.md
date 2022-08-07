##### Promise.resolve：

实现 resolve 静态方法有三个要点:

- 传参为一个 Promise, 则直接返回它。
- 传参为一个 thenable 对象，返回的 Promise 会跟随这个对象，`采用它的最终状态`作为`自己的状态`。
- 其他情况，直接返回以该值为成功状态的promise对象。

```js
Promise.resolve = (param) => {
  if(param instanceof Promise) return param;
  return new Promise((resolve, reject) => {
    if(param && param.then && typeof param.then === 'function') {
      // param 状态变为成功会调用resolve，将新 Promise 的状态变为成功，反之亦然
      param.then(resolve, reject);
    }else {
      resolve(param);
    }
  })
}
```

