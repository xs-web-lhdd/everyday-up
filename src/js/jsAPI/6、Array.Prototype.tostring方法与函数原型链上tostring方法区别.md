##### 6、Array.Prototype.tostring方法与函数原型链上tostring方法区别？
[一文 KO 这个知识点](https://blog.csdn.net/Mr_28/article/details/123395917)


- Array.prototype.toString() 方法返回一个表示指定数组及其元素的字符串。作用等同于使用不传参的 join 方法，但这两个方法都不能对包含 Symbol 类型的值的数据进行转换
```js
[1, '2', true, undefined, null, {}, [], () => 1].toString()  // "1,2,true,,,[object Object],,() => 1"
[1, '2', true, undefined, null, {}, [], () => 1].join()  // "1,2,true,,,[object Object],,() => 1"
[1, '2', true, undefined, null, {}, [], () => 1, Symbol('1')].toString()  // Uncaught TypeError: Cannot convert a Symbol value to a string
// 需要注意的是，如果子项是数组，无论是一层还是多层，都会被展开，如：
[1, [2, [3, [4]]]].toString() // "1,2,3,4"
```

- Object 的原型对象上的 toString 方法，这个方法会返回一个表示对象的字符串，返回的格式是 [object type]，其中的 type 指代的是具体的数据类型，如下示例代码所示：
```js
Object.prototype.toString.call({})  // "[object Object]"
Object.prototype.toString.call([])  // "[object Array]"
Object.prototype.toString.call(null)  // "[object Null]"
Object.prototype.toString.call(undefined)  // "[object Undefined]"
Object.prototype.toString.call('')  // "[object String]"
Object.prototype.toString.call(0)  // "[object Number]"
Object.prototype.toString.call(true)  // "[object Boolean]"
Object.prototype.toString.call(new Date())  // "[object Date]"
Object.prototype.toString.call(new Error())  // "[object Error]"
Object.prototype.toString.call(() => {})  // "[object Function]"
Object.prototype.toString.call(Symbol('s'))  // "[object Symbol]"
Object.prototype.toString.call(new Promise((resolved) => {resolved()}))  // "[object Promise]"
Object.prototype.toString.call(new Set([1, 2]))  // "[object Set]"
Object.prototype.toString.call(new Map([['a', 1]]))  // "[object Map]"
Object.prototype.toString.call(new ArrayBuffer())  // "[object ArrayBuffer]"
Object.prototype.toString.call(/1/)  // "[object RegExp]"

class C {}
Object.prototype.toString.call(C) // "[object Function]"
Object.prototype.toString.call(new C()) // "[object Object]"
```


##### 全局函数 toString 和 Object.prototype.toString 有什么区别？
- 两者其实是同一个函数题，也就是 toString 是继承 Object.prototype.toString 的
```js
toString === Object.prototype.toString // true
```

- 主要区别就是 toString() （注意这里是调用） 和 Object.prototype.toString() （注意这里也是调用） 里面的 `this` 不一致
其中 Object.prototype.toString 是在严格模式下执行的，所以 `toString() ---> '[object Undefined]'` 严格模式下独立调用 this 是 `undefined` ， `Object.prototype.toString() ---> '[object Object]'`

可以来看一个例子来说明这个情况：
```js
toString === window.toString // true
toString() // '[object Undefined]'
window.toString() // '[object Window]'
```