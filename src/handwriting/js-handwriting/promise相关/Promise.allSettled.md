##### Promise.allSettled：

所有 Promise 的状态都变化了，那么新返回一个状态是 fulfilled 的 Promise，且它的值是一个数组，数组的每项由所有 Promise 的值和状态组成的对象；

如果有一个是 pending 的 Promise，则返回一个状态是 pending 的新实例；

```js
    Promise.allSettled = function (promiseArr) {
      let result = [], count = 0
      return new Promise((resolve, reject) => {
        promiseArr.forEach((p, index) => {
          Promise.resolve(p).then(res => {
            count++
            result[index] = {
              status: 'fulfilled',
              value: res
            }
            if (count === promiseArr.length) {
              resolve(result)
            }
          }, err => {
            count++
            result[index] = {
              status: 'rejected',
              value: err
            }
            if (count === promiseArr.length) {
              resolve(result)
            }
          })
        });
      })
    }
```
```js
// 测试用例：
    function createPromise(res, timer) {
      return new Promise((resolve) => {
        setTimeout(() => resolve(res), timer * 1000)
      })
    }

    var arr = [1, createPromise('pppp', 4), createPromise('qqq', 1), 2]


    Promise.allSettled(arr).then(res => {
      console.log(res);
    })
```

