##### Number.isNaN() 与 isNaN() 的区别？

[MDN官方介绍](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN)

全局isNaN() 对于非数字会调用Number()转为数字。

和全局函数 [`isNaN()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN) 相比，`Number.isNaN()` 不会自行将参数转换成数字，只有在参数是值为 `NaN` 的数字时，才会返回 `true`。

```js
const name = 'Lydia Hallie'
const age = 21

console.log(Number.isNaN(name)); // false
console.log(Number.isNaN(age)); // false

console.log(isNaN(name)); // true 原本不是 NaN，因为转数字转成 NaN，然后返回 true
console.log(isNaN(age)); // false
```

