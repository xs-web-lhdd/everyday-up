##### 3、手写 bind：
- 缺陷：没有考虑 bind 返回值通过 new 调用的情况
```js
Function.prototype.myBind = function (target, ...outArgs) {
  target = target || window // 处理边界条件
  const symbolKey = Symbol()
  target[symbolKey] = this

  return function (...innerArgs) { // 返回一个函数
  const res = target[symbolKey](...outArgs, ...innerArgs) // outArgs和innerArgs都是一个数组，解构后传入函数
    delete target[symbolKey]
    return res
  }
}
```
[三元大佬的实现](https://juejin.cn/post/6844903986479251464#heading-40)
[polifill](https://www.jianshu.com/p/2832341dd068)
[死磕 36 道手写题中 bind](https://juejin.cn/post/6946022649768181774#heading-29)