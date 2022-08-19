##### 14、try...catch 可以捕获那些 js 错误？
[try...catch 可以捕获 js 错误](https://zhuanlan.zhihu.com/p/142932660)

结论： 能被 try catch 捕捉到的异常，必须是在报错的时候，线程执行已经进入 try catch 代码块，且处在 try catch 里面，这个时候才能被捕捉到。

在线程前（词法检查错误）不会被捕获，在线程后（异步：setTimeout）不会捕获到错误。

> 注意：捕获不到 Promise 错误，Promise 很强大，不用担心异常会往上抛（内部都已经 try...catch 过了）！我们只需要给 Promise 增加 Promise#catch 就 OK 了