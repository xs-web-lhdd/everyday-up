##### 2、手写 apply：
```js
Function.prototype.myApply = function (context, args) {
  // 不传参时就是绑 window
  context = context || window
  // 防止与 context 身上属性冲突
  const symbolKey = Symbol()
  context[symbolKey] = this
  const res = context[symbolKey](...args)
  delete context[symbolKey]

  return res
}
```