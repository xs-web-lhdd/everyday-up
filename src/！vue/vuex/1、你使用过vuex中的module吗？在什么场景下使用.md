### 1、你使用过 vuex 中的 module 吗？在什么场景下使用？

#### 分析：
这是基本应用能力考察，稍微上点规模的项目都要拆分vuex模块便于维护。

#### 官方文档：
[官网](https://vuex.vuejs.org/zh/guide/modules.html)
```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}
const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}
const store = createStore({
  modules: {
    a: moduleA,
    b: moduleB
  }
})
// 需要命名空间：
store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
// 不需要命名空间：
store.getters.c // -> moduleA里的getters
store.commit('d') // -> 能同时触发子模块中同名mutation
store.dispatch('e') // -> 能同时触发子模块中同名action
```


#### 思路：
- 概念和必要性
- 怎么拆
- 使用细节
- 优缺点


#### 回答范例：
- 用过module，项目规模变大之后，单独一个store对象会过于庞大臃肿，通过模块方式可以拆分开来便于维护
- 可以按之前规则单独编写子模块代码，然后在主文件中通过modules选项组织起来：createStore({modules:{...}})
- 不过使用时要注意访问子模块状态时需要加上注册时模块名：store.state.a.xxx，但同时getters、mutations和actions又在全局空间中，使用方式和之前一样。如果要做到完全拆分，需要在子模块加上namespace选项，此时再访问它们就要加上命名空间前缀。
- 很显然，模块的方式可以拆分代码，但是缺点也很明显，就是使用起来比较繁琐复杂，容易出错。而且类型系统支持很差，不能给我们带来帮助。pinia显然在这方面有了很大改进，是时候切换过去了。

#### 可能的追问:
- 用过pinia吗？都做了哪些改善？（pinia 还没咋用过，先打个 TODO:）