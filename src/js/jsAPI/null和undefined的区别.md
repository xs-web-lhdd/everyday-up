##### null和undefined的区别？

1、null是关键字，undefined不是关键字

2、使用Number进行类型转换的时候null是0，undefined是NaN

3、undefined本质是window的一个属性，而null是一个对象

4、用途不同：

​	null表示没有对象，即不应该有值，经常作为函数的参数，或作为原型链的终点

​	undefined表示缺少值，即应该是有值的但是还没有赋值，变量提升时默认就是undefined、函数参数未提供参数的时候也是undefined、函数返回默认值就是undefined、对象属性没有赋值的时候也是undefined

5、null表示一个"无"的对象，也就是该处不应该有值；而undefined表示未定义。

##### 联系：

1、转为布尔类型值得时候，两者都是false

2、两者进行比较得时候，全等时不等，双等时相等

##### undefined 和 undeclared 的区别：
undefined是声明了但是未初始化，而undeclared是指变量未声明。
```js
var a;
typeof a; //undefined  
typeof b; //undefined  
```
而typeof 在处理undeclared和undefined变量时返回的都是undefined，这是因为typeof底层实现的时候加入了安全防范机制，所以typeof 来判断未声明变量时不会报错。

