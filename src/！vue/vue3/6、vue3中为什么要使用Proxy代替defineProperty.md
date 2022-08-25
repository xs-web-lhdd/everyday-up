### 6、vue3 中为什么使用 Proxy 代替 defineProperty?

#### 回答：
- js 中做属性拦截常见的方式有三种：defineProperty、getter/setter、Proxy
- vue2 中用的 defineProperty ，因为当初 vue 创建的时候还没 Proxy。由于该 API 存在一些局限性，比如对于数组的拦截有问题，为此vue专门为数组响应式做一套实现，另外不能拦截那些新增、删除的属性，所以出现 vue.$set 和 vue.$delete，最后 defineProperty 方案在初始化的时候需要深度递归遍历对处理的对象都进行拦截，明显增加初始化时间。
- 以上亮点在 Proxy 出现以后就迎刃而解，不仅可以实现数组的拦截，还能对 Set、Map 
实现拦截，另外 Proxy 的拦截也是局域懒处理的，如果用户没有访问嵌套对象，那么不会拦截，这就让初始化速度和内存都改善了
- 当然 Proxy 也有兼容性问题，比如 IE，不过现在没有了因为 IE 没了哈哈哈哈哈