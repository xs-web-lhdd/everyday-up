##### new 操作符做了哪些事情


MDN 中对 new 的描述: 使用 new 来构建函数，会执行如下四部操作：

+ 创建一个空的简单 JavaScript 对象（即 {} ）；
+ 为步骤1新创建的对象添加属性 __proto__ ，将该属性链接至构造函数的原型对象 ；
+ 将步骤1新创建的对象作为 this 的上下文 ；
+ 如果该函数没有返回对象（这个对象是所有对象，包括函数对象、数组对象、错误对象那种），则返回 this 。

> 特例：
>
> 1、如果返回值是 null，new 出来是实例对象

#### 例子:

##### 使用new调用函数，而这个函数中有return，那它return出来的是什么?

**构造函数有返回值的情况：当构造函数返回值为对象时，直接返回这个对象；否则返回new创建的对象**

下面为测试部分

###### 返回值为基本类型

假设构造函数返回值为一个基本类型，我们来看一下最后的返回结果：

```js
function Thin_User(name, age) {
    this.name = name;
    this.age = age;
    return 'i will keep thin forever';
}

Thin_User.prototype.eatToMuch = function () {
    console.log('i eat so much, but i\'m very thin!!!');
}

Thin_User.prototype.isThin = true;

const xiaobao = new Thin_User('zcxiaobao', 18);
console.log(xiaobao.name);   // zcxiaobao
console.log(xiaobao.age);    // 18
console.log(xiaobao.isThin); // true
// i eat so much, but i'm very thin!!!
xiaobao.eatToMuch(); 
```

最后的返回结果好像受到任何干扰，难道构造函数不会对返回值进行处理吗？
不急，我们来接着测试一下返回值为对象的情况。

###### 返回值为对象

```js
function Thin_User(name, age) {
    this.name = name;
    this.age = age;
    return {
        name: name,
        age: age * 10,
        fat: true
    }
}

Thin_User.prototype.eatToMuch = function () {
    // 白日做梦吧，留下肥胖的泪水
    console.log('i eat so much, but i\'m very thin!!!');
}

Thin_User.prototype.isThin = true;

const xiaobao = new Thin_User('zcxiaobao', 18);
// Error: xiaobao.eatToMuch is not a function
xiaobao.eatToMuch();
```
