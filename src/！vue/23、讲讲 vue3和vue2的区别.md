### 23、vue2 和 vue3 的区别：
[vue3有了解过吗？能说说跟vue2的区别吗？--- 看着一篇就够了，非常详细](https://vue3js.cn/interview/vue/vue3_vue2.html#%E4%B8%80%E3%80%81vue3%E4%BB%8B%E7%BB%8D) 偏向 API ，原理没讲（也比较多感觉不讲也正常）

[这位牛友也不错！](https://www.nowcoder.com/discuss/1021587?channel=-1&source_id=discuss_terminal_nctrack&trackId=undefined)

#### 速度更快：
重写了虚拟 DOM 实现（这块还不清楚）
编译模板的优化
更高效的组件初始化

#### 体积更小：
通过 webpack 的 treeshaking，可以将无用模块剪掉，仅打包需要的
- 对开发人员，可以实现更多功能，也不怕整体体积过大
- 对使用者，打包出来的包体积变小了

#### 更灵活和更容易维护：
Composition API 出现了
- 可以 option 模块一起使用
- 灵活的代码复用
- 响应式模块可以和其他系统一起使用
- 更好的 Typescript 支持

#### 扩展性更好：
- 出现自定义渲染器，方便跨平台

#### 更易使用：
- 一些响应式的 API 暴露出来，observable、effect 那种


#### 新增特性：
- teleport 
- fragement
- compositon API
- 自定义渲染器

#### API 变更（涉及原理）：
- 新增 emits 对应 props
- 组件上 v-model 用法已更改
- 在同一元素上使用的 v-if 和 v-for 优先级已更改
- 自定义指令 API 已更改为与组件生命周期一致（之前是 bind unbind update componentUpdate mounted）
- 一些转换 class 被重命名了；过渡上的
  - v-enter -> v-enter-from
  - v-leave -> v-leave-from
- 组件 watch 选项和实例方法 $watch不再支持复杂操作了
- destroyed 生命周期选项被重命名为 unmounted
- beforeDestroy 生命周期选项被重命名为 beforeUnmount

#### 移除：
- $on，$off和$once 实例方法
- 过滤filter