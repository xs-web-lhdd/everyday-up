### 5、vue3 的性能提升在那几个方面？
#### 回答：
我从代码、编译、打包三方面介绍 vue3 性能方面的提升
- 代码层面性能优化主要体现在全新的响应式 API，基于 Proxy 实现，初始化时间和内存大幅改进。
- 编译层面做了更多编译优化处理，比如：静态提升、动态内容标记、事件缓存、区块等，可以有效跳过大量 diff 过程
- 打包时更好的支持 tree-shaking，因此整体体积更小，加载更快