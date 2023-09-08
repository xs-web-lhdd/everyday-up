### 20、vue 的声明周期：
[国内 vue 中文网](https://vue3js.cn/interview/vue/lifecycle.html#%E4%B8%89%E3%80%81%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%95%B4%E4%BD%93%E6%B5%81%E7%A8%8B)


##### setup 和 created 谁先执行？
setup 先执行

##### setup 中为什么没有 beforeCreate 和 created ？
因为 setup 执行时机比 beforeCreate 和 created 早，在 setup 是时我们就已经知道组件实例已经创建好了，既然已经创建好了那么执行这两个钩子函数还有什么意义呢？而且一个滞后的东西放入前面又有什么意义呢？我们声明周期的意义就是提前先把东西埋好然后到对应时间点再执行的，所以岂不是又没意义了对于 beforeCreate 和 created 来说。

那为什么还要有 beforeCreate 和 created ？
因为是为了 options API 而准备的。


在 vue3 中 beforeCreate 和 created 是在 `applyOptions` 中的,`applyOptions`是在 setup 函数完完整整执行完之后才执行的


##### render 函数在那个阶段生效？
在 vue2 中 render 函数在 beforeMount 之前生效，在 updated 之前重新生成新的渲染函数然后进行进行虚拟 DOM 的 diff 算法


##### 虚拟 DOM 比较在那个阶段？
在 vue（2 3 都是） 中是在 updated 之前进行 diff

##### ajax 发送请求在 vue 的哪个生命周期发送？


##### vue 的生命周期是否能做箭头函数？
vue2 不可以，因为 this 的缘故，vue3 可以


##### window.onload 相当于哪个生命周期？created 相当于那个生命周期？（？？？什么垃圾问题）
mounted 、 window.onreadey