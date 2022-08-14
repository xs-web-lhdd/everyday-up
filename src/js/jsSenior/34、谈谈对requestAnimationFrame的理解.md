##### 谈谈对requestAnimationFrame的理解:
看 35、题之后再来看这题会好很多！
可以看 35 题字节佬文章中的 requestAnimationFrame


##### requestAnimationFrame 是微任务还是宏任务？：
- requestAnimationFrame和requestIdleCallback是和宏任务性质一样的任务，只是他们的执行时机不同而已。也有人说它们既不是宏任务也不是微任务，其实我们不必纠结这个，我们所要做的就是知道他们的执行时机就好。

- requestAnimationFrame回调的执行与task和microtask无关，而是与浏览器是否渲染相关联的。

- 它是在浏览器渲染前，在微任务执行后执行。
- 很可能在宏任务之后不调用。




