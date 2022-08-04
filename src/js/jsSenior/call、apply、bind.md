##### call、apply、bind区别
1. call() 和apply()的第一个参数相同，就是指定的对象。这个对象就是该函数的执行上下文。
2. call()和apply()的区别就在于，两者之间的参数。
3. call()在第一个参数之后的 后续所有参数就是传入该函数的值。
4. apply() 只有两个参数，第一个是对象，第二个是数组，这个数组就是该函数的参数。
5. bind() 方法和前两者不同在于： bind() 方法会返回执行上下文被改变的函数而不会立即执行，而前两者是	直接执行该函数。他的参数和call()相同。

> 注：bind的过的函数再bind是不起作用的，所以bind只第一次起作用

例：

```js
var obj = {
  name: 'zwj',
  getName: function () {
    return this.name
  }
}

function getGloabalName() {
  return this.name
}

var name = 'xxx'

var foo = {
  name: 'ppppp'
}

var a = getGloabalName.bind(foo) // 第一次bind，会起作用
var b = a.bind(obj) // 第二次bind，这次 bind 是不起作用的
console.log(a());
console.log(b());
```

