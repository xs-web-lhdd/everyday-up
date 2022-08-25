##### vue 中组件通信：
[国内 vue 中文社区](https://vue3js.cn/interview/vue/communication.html#%E4%B8%89%E3%80%81%E7%BB%84%E4%BB%B6%E9%97%B4%E9%80%9A%E4%BF%A1%E7%9A%84%E6%96%B9%E6%A1%88)

#### 回答范例：
1、组件通信常用方式有以下8种：
- props
- $emit/$on（已删除）
- $children（已删除）/$parent
- $attrs/$listeners（已删除）
- ref
- $root
- eventbus（vue3中已经移除）
- vuex

> 注意vue3中废弃的几个API
https://v3-migration.vuejs.org/breaking-changes/children.html
https://v3-migration.vuejs.org/breaking-changes/listeners-removed.html
https://v3-migration.vuejs.org/breaking-changes/events-api.html#overview


2、根据组件之间关系讨论组件通信最为清晰有效
父子组件：
props/$emit/$parent/ref/$attrs

兄弟组件：
$parent/$root/eventbus/vuex

跨层级关系：
eventbus/vuex/provide+inject


##### vue 中兄弟组件通信：

###### 总结：
- 父子关系的组件数据传递选择 props ，与 $emit进行传递，也可选择ref
- 兄弟关系的组件数据传递可选择$bus，其次可以选择$parent进行传递
- 祖先与后代组件数据传递可选择attrs与listeners或者 Provide与 Inject
- 复杂关系的组件数据传递可以通过vuex存放共享的变量

##### provide 和 inject 绑定是不是可响应的?
不是！

如何做到响应式？
