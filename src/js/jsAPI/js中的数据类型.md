##### JavaScript的数据类型

1. 基本数据类型：共有7种

```js
Boolean Number String undefined null Bigint Symbol
```

Symbol ： ES6 引入的一种新的原始值，表示独一无二的值，主要为了解决属性名冲突问题。
Bigint ：ES2020 新增加，是比 Number 类型的整数范围更大。

2. 引用数据类型：1种

```js
Object对象(包括普通Object、Function、Array、Date、RegExp、Math)
```

##### 数据类型怎么存储：

基本类型存储在栈中，引用类型存储在堆中

>  注意一种特殊情况：闭包中的基本类型也是存储在堆中

##### 堆和栈你怎么理解？

从数据结构的角度出发：堆是在物理结构上是一个优先队列，在逻辑结构上是一个堆；栈是先进后出，js中常常用数组来模拟他的方法，比如pop、push

###### 延申：能不能实现一个优先队列？

从操作系统的角度出发：堆的大小不固定，会在栈中存储堆的地址，也可以说是引用。栈的大小固定。

##### 判断数据类型的方法

###### typeof

> 优点：能够快速区分基本数据类型 缺点：不能将Object、Array和Null区分，都返回object

1. `typeof`的作用？

   区分数据类型，可以返回7种数据类型：`number、string、boolean、undefined、object、function` ，以及 `ES6` 新增的 `symbol 和 bigInt`

2. `typeof` 能正确区分数据类型吗？

   不能。对于原始类型，除 `null` 都可以正确判断；对于引用类型，除 `function` 外，都会返回 `"object"`

3. `typeof` 注意事项

   + `typeof` 返回值为 `string` 格式，注意类似这种考题: `typeof(typeof(undefined)) -> "string"`
   + `typeof` 未定义的变量不会报错，返回 `"undefiend"` 
   + `typeof(null) -> "object"`: 遗留已久的 `bug`
   + `typeof`无法区别数组与普通对象: `typeof([]) -> "object"`
   + `typeof(NaN) -> "number"`

###### instanceof

> 优点：能够区分Array、Object和Function，适合用于判断自定义的类实例对象 缺点：Number，Boolean，String基本数据类型不能判断

1. `instanceof` 判断对象的原型链上是否存在构造函数的原型。只能判断引用类型。
2. `instanceof` 常用来判断 `A` 是否为 `B` 的实例

###### Object.prototype.toString.call()

> 优点：精准判断数据类型 缺点：写法繁琐不容易记，推荐进行封装后使用

```js
toString.call(()=>{})       // [object Function]
toString.call({})           // [object Object]
toString.call([])           // [object Array]
toString.call('')           // [object String]
toString.call(22)           // [object Number]
toString.call(undefined)    // [object undefined]
toString.call(null)         // [object null]
toString.call(new Date)     // [object Date]
toString.call(Math)         // [object Math]
toString.call(window)       // [object Window]
```

 ##### instanceof原理，模拟实现

什么是instanceof？你能模拟实现一个instanceof吗？

1. `instanceof` 判断对象的原型链上是否存在构造函数的原型。只能判断引用类型。
2. `instanceof` 常用来判断 `A` 是否为 `B` 的实例

```js
// A是B的实例，返回true，否则返回false
// 判断A的原型链上是否有B的原型
A instaceof B
```

3. 模拟实现 `instanceof`

思想：沿原型链往上查找

```js
function instance_of(Case, Constructor) {
    // 基本数据类型返回false
    // 兼容一下函数对象
    if ((typeof(Case) != 'object' && typeof(Case) != 'function') || Case == 'null') return false;
    let CaseProto = Object.getPrototypeOf(Case);
    while (true) {
        // 查到原型链顶端，仍未查到，返回false
        if (CaseProto == null) return false;
        // 找到相同的原型
        if (CaseProto === Constructor.prototype) return true;
        CaseProto = Object.getPrototypeOf(CaseProto);
    }
}
```

##### typeof 与 instanceof 的区别

typeof与instanceof都是判断数据类型的方法，区别如下：

1. typeof会返回一个变量的基本类型，instanceof返回的是一个布尔值
2. instanceof 可以准确地判断复杂引用数据类型，但是不能正确判断基础数据类型
3. 而 typeof 也存在弊端，它虽然可以判断基础数据类型（null 除外），但是引用数据类型中，除了 function 类型以外，其他的也无法判断

> 通用检测数据类型，可以采用Object.prototype.toString，调用该方法，统一返回格式“[object Xxx]” 的字符串

##### typeof为什么对null错误的显示

这只是 JS 存在的一个悠久 Bug。在 JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象然而 null 表示为全零，所以将它错误的判断为 object


> 注意：Symbol.toStringTag 可以改变 `Object.prototype.toSting.call(xxx)` 的返回值。
> Symbol.hasInstance 会改变 instanceof 的行为：
MDN解释：**Symbol.hasInstance**用于判断某对象是否为某构造器的实例。因此你可以用它自定义 instanceof 操作符在某个类上的行为。
```js
function Xiao(){}
const xiao = new Xiao
xiao instanceof Xiao // true ====> Xiao[Symbol.hasInstance](xiao)
```
这个是内部的方法，不支持重写，当然，我们可以在原型上改写。
```js
Object.definePrototype(Xiao, Symbol.hasInstance, {
   value: (v) => Boolean(v)
}) // 注意这里 value 一定是个函数，因为要调用！
const x = new Xiao
x instanceof Xiao //true
0 instanceof Xiao //false
1 instanceof Xiao //true
```

错误示范：
```js
function Xiao() {}

Xiao[Symbol.hasInstance] = function () {
  return false
} // 不起作用的

const obj = new Xiao()

console.log(obj instanceof Xiao); // true 不起作用的
```