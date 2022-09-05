### 46、说说 callback、promise、generate、async await ？
[掘金好问](https://juejin.cn/post/6987549240436195364#heading-33)

- callback:
  callback 是最开始用来编写异步代码的书写方式，但是有很多问题：
    - 回调函数环境的变化会导致 this 指向发生变化 
    - 使用回调函数来处理多个互相依赖的异步很容易产生回调地狱
    - 对于 JavaScript 中标准的异步 API 可能无法通过在外部进行 try...catch... 的方式进行错误捕获：

- Promise：
  Promise 基于有限状态机来进行异步问题的处理
    - Promise 对象的执行状态不受外界影响。Promise 对象的异步操作有三种状态： pending（进行中）、 fulfilled（已成功）和 rejected（已失败） ，只有 Promise 对象本身的异步操作结果可以决定当前的执行状态，任何其他的操作无法改变状态的结果
    - Promise 对象的执行状态不可变。Promise 的状态只有两种变化可能：从 pending（进行中）变为 fulfilled（已成功）或从 pending（进行中）变为 rejected（已失败）
  > 有限状态机提供了一种优雅的解决方式，异步的处理本身可以通过异步状态的变化来触发相应的操作，这会比回调函数在逻辑上的处理更加合理，也可以降低代码的复杂度。
    - Promise 有 catch 方法更好的捕获错误
    - 还有许多其他 API，使得处理异步更加强大（all、allSettle、race、any）
  `缺点`：
    - 无法取消 Promise 的执行
    - 无法在 Promise 外部通过 try...catch... 的形式进行错误捕获（Promise 内部捕获了错误）

- generator：
  它使得异步代码的设计和执行看起来和同步代码一致。
    - Generator 函数内部的异步代码执行看起来和同步代码执行一致，非常利于代码的维护
    - Generator 函数内部的执行逻辑和`相应的状态变化（next）逻辑解耦`，降低了代码的复杂度
  > 注意：返回的是一个迭代器，并且 yield 只支持 Promise 和 chunck 函数

- async await：
  是 generator 的语法糖，是解决异步的终极方案：
    - 内置执行器：Generator 函数需要设计手动执行器或者通用执行器（例如 Co 执行器）进行执行，Async 语法则内置了自动执行器，设计代码时无须关心执行步骤
    - yield 命令无约束：在 Generator 中使用 Co 执行器时 yield 后必须是 Promise 对象或者 Thunk 函数，而 Async 语法中的 await 后可以是 Promise 对象或者原始数据类型对象、数字、字符串、布尔值等（此时会对其进行 Promise.resolve() 包装处理） 
    - 返回 Promise： async 函数的返回值是 Promise 对象（返回原始数据类型会被 Promise 进行封装）， 因此还可以作为 await  的命令参数，相对于 Generator 返回 Iterator 遍历器更加简洁实用
    - 可以通过 try...catch... 在 async 函数里进行局部错误捕获


