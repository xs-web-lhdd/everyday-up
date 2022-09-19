### 12、nexttick了解吗？


#### 答题思路：
- nextTick是做什么的？
- 为什么需要它呢？
- 开发时何时使用它？抓抓头，想想你在平时开发中使用它的地方
- 下面介绍一下如何使用nextTick
- 原理解读，结合异步更新和nextTick生效方式，会显得你格外优秀



#### 回答范例：
1、nextTick是等待下一次 DOM 更新刷新的工具方法。

2、Vue有个异步更新策略，意思是如果数据变化，Vue不会立刻更新DOM，而是开启一个队列，把组件更新函数保存在队列中，在同一事件循环中发生的所有数据变更会异步的批量更新。这一策略导致我们对数据的修改不会立刻体现在DOM上，此时如果想要获取更新后的DOM状态，就需要使用nextTick。

3、开发时，有两个场景我们会用到nextTick：
- created中想要获取DOM时；
- 响应式数据变化后获取DOM更新后的状态，比如希望获取列表更新后的高度。

4、nextTick签名如下：`function nextTick(callback?: () => void): Promise<void>`
所以我们只需要在传入的`回调函数`中访问最新DOM状态即可，或者我们可以`await nextTick()`方法返回的Promise之后做这件事。

5、在Vue内部，nextTick之所以能够让我们看到DOM更新后的结果，是因为我们传入的callback会被添加到队列刷新函数(flushSchedulerQueue)的后面，这样等队列内部的更新函数都执行完毕，所有DOM操作也就结束了，callback自然能够获取到最新的DOM值。


#### 知其所以然：
1、源码解读：
组件更新函数入队：
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/renderer.ts#L1547-L1548

入队函数：
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/scheduler.ts#L84-L85

nextTick定义：
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/scheduler.ts#L58-L59


### 如果父组件和子组件的 mounted 中都用到了 nextTick，会怎么执行，都会执行吗？