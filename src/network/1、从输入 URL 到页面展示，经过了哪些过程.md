##### 从输入 URL 到页面展示，经过了哪些过程？

先看这两个：

[从URL输入到页面展现到底发生什么？ - 掘金 (juejin.cn)](https://juejin.cn/post/6844903784229896199)

[从输入URL开始建立前端知识体系 - 掘金 (juejin.cn)](https://juejin.cn/post/6935232082482298911)
[这个牛逼---从输入URL到页面加载的过程？如何由一道题完善自己的前端知识体系！ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/34453198?group_id=957277541711540224)

[字节面试被虐后，是时候搞懂 DNS 了 - 掘金 (juejin.cn)](https://juejin.cn/post/6990344840181940261)  说实话B站一搜还是很好的

https://www.bilibili.com/video/BV1JZ4y1f7tU?from=search&seid=9229912515884661837&spm_id_from=333.337.0.0



###### 最终版本：看完总结出来估计要一天！焯
[网络通信篇](https://juejin.cn/post/6844904132071915527)
[页面渲染篇](https://juejin.cn/post/6844904134307495943)

[合成篇](https://juejin.cn/post/6844904155077672968)

[牛友篇](https://www.nowcoder.com/discuss/975345?channel=-1&source_id=discuss_terminal_nctrack&trackId=undefined)
##### 如何寻址？


#### DNS解析时，怎么让他预解析
`<link rel="dns-prefetch" href="//www.zhihu.com"> `

#### 域名服务器还有什么方法解析域名；DNS跟底层哪些协议相关？

#### HTTP建立连接过程，说一些SSL，怎么实现的，其中的密码学相关知识（问了超多）？

#### 怎么实现一个文件的提前加载

##### 你刚刚说多进程浏览器，你说说启动一个页面有什么进程？
打开一个页面至少要有四个进程：浏览器进程、网络进程、渲染进程、GPU进程

- 浏览器进程：主要负责界⾯显⽰、⽤⼾交互、⼦进程管理，同时提供存储等功能。

- 渲染进程：核⼼任务是将 HTML、CSS 和 JavaScript 转换为⽤⼾可以与之交互的⽹⻚，排版引擎Blink和JavaScript引擎V8都是运⾏在该进程中，默认情况下，Chrome会为每个Tab标签创建⼀个渲染进程。出于安全考虑，渲染进程都是运⾏在沙箱模式下。

- GPU进程：其实，Chrome刚开始发布的时候是没有GPU进程的。⽽GPU的使⽤初衷是为了实现3D CSS的效果，只是随后⽹⻚、Chrome的UI界⾯都选择采⽤GPU来绘制，这使得GPU成为浏览器普遍的需求。最后，Chrome在其多进程架构上也引⼊了GPU进程。

- ⽹络进程：主要负责⻚⾯的⽹络资源加载，之前是作为⼀个模块运⾏在浏览器进程⾥⾯的，直⾄最近才独⽴出来，成为⼀个单独的进程。

- 插件进程：主要是负责插件的运⾏，因插件易崩溃，所以需要通过插件进程来隔离，以保证插件进程崩溃不会对浏览器和⻚⾯造成影响。

##### 服务端返回的第一的文件是啥？
index.html