##### 了解过bigInt吗?

 [神三元的文章](https://juejin.cn/post/6844903974378668039#heading-6)

##### 介绍一下symbol：

参考答案: [symbol---讲的非常好](https://juejin.cn/post/6844903703242080263)

##### 为什么 Symbol、bigInt 不能 new ？
js 高级程序设计：这样做是为了避免创建 Symbol 原始值包装对象
我觉得：新增的 Symbol、BigInt 没有必要向开发者提供创建对应原始值对象的 API。一般也不会使用，多此一举。

如果非要创建可以通过包一层 Object
```js
let mySymbol = Symbol() 
mySymbol // Symbol()
typeof mySymbol // "symbol"
let myWrappedSymbol = Object(mySymbol)
myWrappedSymbol // Symbol {Symbol()}
typeof myWrappedSymbol // "object"
```

##### Symbol、BigInt内部是怎么检测用户使用了new来调用的？
