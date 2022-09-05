### 6、知道 pinia 吗？
[B 站技术胖，API 级别很好](https://www.bilibili.com/video/BV1oP4y1w7pz?spm_id_from=333.337.search-card.all.click)
[官方文档也很清晰，没啥说的](https://pinia.web3doc.top/core-concepts/actions.html)
[vuex](https://vuex.vuejs.org/zh/guide/modules.html#%E7%BB%99%E6%8F%92%E4%BB%B6%E5%BC%80%E5%8F%91%E8%80%85%E7%9A%84%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)

[手写 pinia](https://www.bilibili.com/video/BV1bS4y1W7Ed?spm_id_from=333.337.search-card.all.click&vd_source=605eaae8506df36fc86fca9b2b498d80)

五大优点：
1、vue3、vue2 都支持（官网的在 setup 中使用，不在 setup 中使用就是）
2、抛弃 Mutation 模块、只有 state、getters 和 actions（state还是数据，getters 还是类似计算属性，actions 进行异步操作）
3、不需要嵌套模板，符合 vue3 的composition API
4、完整的 TS 支持（类型推断更好，但是注意如果 getters 中使用 this，而不是函数参数 state 时要定义返回类型，因为推断不出来）
5、代码更加简洁（用法真的简洁了很多）

