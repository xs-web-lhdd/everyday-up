### 23、addEventListener 和 onclick 有什么区别？
1.addEventListener可以对同一个元素绑定多个事件，执行顺序从上到下依次执行。而onclick同一个元素只能绑定一个事件，如有多个，后面的事件会覆盖前面的事件。
2.addEventListener的第三个参数为布尔类型，默认为false，也就是执行的冒泡机制，如为true，则执行捕获机制。
4.注册addEventListener事件时不需要写on，而onclick方式则必须加on。
5.在移除事件上，onclick使用的是指针指向null，例如document.onclick = null，而addEventListener则使用的是独有的移除方法removeListener（要使用此方法，addEventListener必须执行的是外部函数或存在函数名，不然则不能使用）
