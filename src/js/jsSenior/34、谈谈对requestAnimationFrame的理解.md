##### 谈谈对requestAnimationFrame的理解:
看 35、题之后再来看这题会好很多！
可以看 35 题字节佬文章中的 requestAnimationFrame

[API 讲解](https://newbyvector.github.io/2018/05/01/2015-05-01/)

#### 优点
- requestAnimationFrame 会把每一帧中的所有 DOM 操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率
- 在隐藏或不可见的元素中，requestAnimationFrame 将不会进行重绘或回流，这当然就意味着更少的 CPU、GPU 和内存使用量
- requestAnimationFrame 是由浏览器专门为动画提供的 API，在运行时浏览器会自动优化方法的调用，并且如果页面不是激活状态下的话，动画会自动暂停，有效节省了 CPU 开销

##### requestAnimationFrame 是微任务还是宏任务？：
- requestAnimationFrame和requestIdleCallback是和宏任务性质一样的任务，只是他们的执行时机不同而已。也有人说它们既不是宏任务也不是微任务，其实我们不必纠结这个，我们所要做的就是知道他们的执行时机就好。

- requestAnimationFrame回调的执行与task和microtask无关，而是与浏览器是否渲染相关联的。

- 它是在浏览器渲染前，在微任务执行后执行。
- 很可能在宏任务之后不调用。




