### 实现 reduce？
牛逼实现参考: 
https://juejin.cn/post/6844903938890661896#heading-3
[三元](https://juejin.cn/post/6844903986479251464#heading-25)
```js
Array.prototype.reduce2 = function(callback, initialValue) {
  if (this == null) {
    throw new TypeError('this is null or not defined')
  }
  if (typeof callback !== "function") {
    throw new TypeError(callback + ' is not a function')
  }
  const O = Object(this)
  const len = O.length >>> 0
  let k = 0, acc
  
  if (arguments.length > 1) {
    acc = initialValue
  } else {
    // 没传入初始值的时候，取数组中第一个非 empty 的值为初始值
    while (k < len && !(k in O)) {
      k++
    }
    if (k > len) {
      throw new TypeError( 'Reduce of empty array with no initial value' );
    }
    acc = O[k++]
  }
  while (k < len) {
    if (k in O) {
      acc = callback(acc, O[k], k, O)
    }
    k++
  }

  return acc
}
```


简易版实现:
细节：`第一次执行回调函数时，不存在“上一次的计算结果”。如果需要回调函数从数组索引为 0 的元素开始执行，则需要传递初始值。否则，数组索引为 0 的元素将被作为初始值 initialValue，迭代器将从第二个元素开始执行（索引为 1 而不是 0）。`
```js
Array.prototype.reduce2 = function (callBack, initialValue) {
  let arr = this
  let pre, length = this.length,
    k
  if (initialValue !== undefined) pre = initialValue, k = 0
  else pre = arr[0], k = 1

  for (let i = k; i < length; i++) {
    pre = callBack(pre, arr[i], i, arr)
  }

  return pre
}
```