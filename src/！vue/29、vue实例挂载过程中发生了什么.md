### 29、vue 实例挂载过程中发生了什么？
#### 分析
挂载过程完成了最重要的两件事：
- 初始化
- 建立更新机制
把这两件事说清楚即可！

#### 回答范例
- 挂载过程指的是app.mount()过程，这个是个初始化过程，整体上做了两件事：初始化和建立更新机制(`setupRenderEffect`)
- 初始化会创建组件实例（`createComponentInstance`）、初始化组件状态（`setupComponent`）、创建各种响应式数据
- 建立更新机制这一步会立即执行一次组件更新函数，这会首次执行组件渲染函数并执行patch将前面获得vnode转换为dom；同时首次执行渲染函数会创建它内部响应式数据和组件更新函数之间的依赖关系，这使得以后数据变化时会执行对应的更新函数。


上面是 vue3 的挂载，下面是 vue2 的挂载：
[国内 vue 爱好者](https://vue3js.cn/interview/vue/new_vue.html#%E4%B8%80%E3%80%81%E5%88%86%E6%9E%90)

#### 知其所以然：
测试代码，test-v3.html

mount函数定义：
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/apiCreateApp.ts#L277-L278

首次render过程：
https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/renderer.ts#L2303-L2304

#### 可能的追问：
- 响应式数据怎么创建？（在 setupComponent 里面）
- 依赖关系如何建立？（暂时还没弄明白）