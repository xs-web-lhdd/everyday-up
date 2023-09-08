### 本目录是前端代码中看代码说输出的题目：

```js
    function f(a) {
      a.x = 1;
      console.log(a.x);
      a = { x: 3 };
      console.log(a.x);
    }
    var a = { x: 0 };
    f(a);
    console.log(a.x);
```
> 知识点：js 的 传参方式
这里面都是按值传参，所以传的是地址，然后 f 里面的 a 开辟的空间的地址与外边 a 的地址一致，但是两者可并不是一个地址，所以 f 内部 a 的栈中地址改变时，外边 a 的栈中地址是不会改变的，所以最后一个 console 结果是 1



```js
async function async1(){
  console.log('async1 start')
  await async2()
  console.log('async1 end')
}
async function async2(){
  console.log('async2')
}
console.log('script start')
setTimeout(function(){
  console.log('setTimeout') 
},0)  
requestAnimationFrame(function(){
    console.log('requestAnimationFrame') 
})
async1();
new Promise(function(resolve){
  console.log('promise1')
  resolve();
}).then(function(){
  console.log('promise2')
})
```
> 微任务和宏任务
涉及 `requestAnimationFrame` 的执行时机
阅读: 
[字节佬讲解](https://zhuanlan.zhihu.com/p/142742003)
[简书一文](https://www.jianshu.com/p/ba5828330aec)



```js
function fun(a, b) {
  console.log(b)
  return {
    fun: function (c) {
      return fun(c, a);
    }
  };
}

var d = fun(0);
d.fun(1);
d.fun(2);
d.fun(3);

var d1 = fun(0).fun(1).fun(2).fun(3);


var d2 = fun(0).fun(1);
d2.fun(2);
d2.fun(3);
```
> 知识点：作用域 访问VO AO
> 注意：return 的 {} 不是作用域！不是作用域！不是作用域！


```js
const person = { name: '代码与野兽' }
Object.defineProperty(person, 'age', { value: 18 })

console.log(person.age) // 18
console.log(Object.keys(person)) ['name']
```
> 知识点：Object.defineProperty 中如果不设置默认都是 false
> { value: 18, writable: false, enumerable: false, configurable: false }



```js
const name = '代码与野兽'
age = 18

console.log(delete name)
console.log(delete age)
console.log(typeof age)
```
> name 没有变成 window 属性，所以 delete name 返回 false
> age 是 window 属性，所以 delete age 返回 true
> typeof undefined 返回 undefined



```js
let person = { name: '代码与野兽' }
const members = [person]
person = null
console.log(members)
// 输出：
// [{
//   name: "代码与野兽"
// }]
```
> 知识点：对象在栈中存的堆中的地址。所以 members 中第一项就是 `地址`，然后 person 在栈中存的也是地址，现在把 person 的地址改成 null，但是 对 members 不影响啊，如果 `members[0] = null`，那么会有影响




```js
const name = '代码与野兽'
console.log(name.padStart(14))
console.log(name.padStart(2))

// "         代码与野兽"
// "代码与野兽"
```
> 解析：
padStart 方法可以在字符串的开头填充空格。
参数是新字符串的总长度，如果这个长度比原来的字符串长度短，那么不会填充。



```js
console.log(parseInt("7"))
console.log(parseInt("7*6"))
console.log(parseInt("7Din"))
// 7
// 7
// 7
```
> 解析：
如果 parseInt 的参数是字符串和数字的组合，那么它会从头开始检查，直到碰到数据类型错误的位置，如果在数据类型错误的位置之前是一个有效的数字，它就会返回数字。


```js
[1, 2, 3, 4].reduce((x, y) => console.log(x, y))

// 1 2
// undefined 3
// undefined 4
```
> 解析：
如果我们不给 reduce 传递初始值，那么 x 会是数组的第一个值，y 是数组的第二个值。



```js
function getUserInfo(one, two, three) {
  console.log(one)
  console.log(two)
  console.log(three)
}

const superHero = '代码与野兽'
const age = 1000

getUserInfo `${superHero} 是 ${age} 岁`
getUserInfo `hello`

// [ '', ' 是 ', ' 岁' ]
// 代码与野兽
// 1000
// [ 'hello' ]
// undefined
// undefined
```
> 我们使用模板字符串语法去调用函数时，第一个参数始终都是分割好的字符串数组（根据 `${}` 分割）。其余的参数是模板表达式的值。


```js
(() => {
  let x, y;
  try {
    throw new Error()
  } catch (x) {
    (x = 1), (y = 2);
    console.log(x)
  }
  console.log(x)
  console.log(y)
})()
```
> try...catch 会延长作用域链（详情见 everyday-up/src/js/jsSenior/32、什么是作用域），所以
在 catch 内有新的作用域 x，然后里面打印时 1，而 y 在 catch 作用域内没有定义，所以 y = 2 修改的是外层作用域（箭头函数中） y 的值，所以 下面是 undefined 2


```js
class Clazz {}

console.log(typeof Clazz)

// "function"
```
> 解析：class 只是语法糖，本质是还是 `函数`


```js
const foo = {
  bar: 1
};
with(foo) {
  var bar = 2
};
console.log(foo.bar);
```
> with 更改作用域，with 的对象会作为 global 对象。在 with 使用 var 等价于 window.[xxx]。而这时 foo 就是那个 window。


```js
// 解构结果
let [foo] = [];
console.log(foo); // undefined

let [bar, foo] = [1];
console.log(bar); // 1
console.log(foo); // undefined
```



```js
var count = 10;

function a() {
  return count + 10;
}

function b() {
  var count = 20;
  return a();
}
console.log(b());
```
> 知识点：作用域，作用域是静态的，在定义的时候就已经确定，而不是调用的时候确定。


看代码：
```js
Promise(a).then(b).catch(c).then(d)没有抛出错误会执行什么

// b里抛出错误会执行什么
// a里抛出错误会执行什么
```


```js
let ls = [1, 2, 3, 4]
let ls1 = ls.map((item) => {}) // 没有返回值就默认返回 undefined
console.log(ls1) // [ undefined, undefined, undefined, undefined ]
```

<!-- 4399 笔试题 -->
```js
setTimeout(() => {
  console.log(1);
}, 100)

new Promise((resolve) => {
  console.log(2);
  resolve()
  console.log(3);
}).then(() => {
  console.log(4);
  setTimeout(() => {
    console.log(6);
  }, 10);
})

console.log(7);
console.log(8);
// 2 3 7 8 4 6 1
```
> 知识点：事件循环（主要是 setTimeout 10 ms在前面，100 ms在后面）


<!-- 4399 笔试题： -->
```js
function Parent() {
  this.a = 1
  this.b = [1, 2, this.a]
  this.c = {
    demo: 3
  }
  this.show = function () {
    console.log(this.a, this.b, this.c.demo);
  }
}

function Child() {
  this.a = 2
  this.change = function () {
    this.b.push(this.a)
    this.a = this.b.length
    this.c.demo = this.a
  }
}


let parent = new Parent()
parent.show()
// 1 [1,2,1] 3
Child.prototype = new Parent()
let child1 = new Child()
let child2 = new Child()
child1.a = 11
child2.a = 12
child1.change()
child2.change()
parent.show()
// 1 [1,2,1] 3
child1.show()
// 4 [1,2,1,11,12] 5
child2.show()
// 5 [1,2,1,11,12] 5
```
> 知识点：原型链


<!-- 4399 笔试题： -->
```js
(function () {
  var x = y = 1
})()

var z;

console.log(z); // undefined
console.log(y); // 1
console.log(x); // x is not defined
```
> 考察作用域：y没有定义，所以会挂载在全局中，z 没赋值所以是 undefined，x 在函数内访问不到


```js
let a
let b = '小明'
console.log(a || b) // '小明'
b = '小红'
console.log(a && b); // undefined
```
> 考察运算符：第一个 log 没有进行短路操作，第二个 log 进行算路操作



```js
// 暂时性死区！
console.log(b);
let b = 0;
console.log(c); // fn

function c() {}
var tmp = 123;
if (true) {
  // 暂时性死区！
  tmp = 'abc';
  let tmp;
}
console.log(tmp);
```
> 考察暂时性死区：`b`（全局作用域） 和 `tmp`（块级作用域） 都是暂时性死区，不能在 let 声明前使用