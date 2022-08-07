##### Promise.any：

空数组或者所有 Promise 都是 rejected，则返回状态是 rejected 的新 Promsie，且值为 AggregateError 的错误；

只要有一个是 fulfilled 状态的，则返回第一个是 fulfilled 的新实例；

其他情况都会返回一个 pending 的新实例；

```js
Promise.any = function (promiseArr) {
  let index = 0
  return new Promise((resolve, reject) => {
    if (promiseArr.length === 0) return Promise.reject(new AggregateError('All promises were rejected'))
    promiseArr.forEach((p, i) => {
      Promise.resolve(p).then(val => {
        resolve(val)

      }, err => {
        index++
        if (index === promiseArr.length) {
          reject(new AggregateError('All promises were rejected'))
        }
      })
    })
  })
}
```

