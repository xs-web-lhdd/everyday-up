##### 9、了解内部的 [[class]] 吗？
[Symbol.toStringTag](https://blog.csdn.net/wu_xianqiang/article/details/121642086)
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toStringTag)
```js
class Class2 {
  get [Symbol.toStringTag]() {
    return "Class2";
  }
}
function test() {}
test[Symbol.toStringTag] = 'pppp'

let a = {
  [Symbol.toStringTag]: '111'
}


console.log(Object.prototype.toString.call(new Class2())); // "[object Class2]"
console.log(Object.prototype.toString.call(a)); // "[object 111]"
console.log(Object.prototype.toString.call(test)); // "[object pppp]"
```

也就是说，这个属性可以用来定制[object Object]或[object Array]中object后面的那个字符串。


Symbol.toStringTag     属性的属性特性：
writable               false
enumerable	           false
configurable	         false