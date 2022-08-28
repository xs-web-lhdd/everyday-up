##### v-if 和 v-show 的区别：
[vue3 中文网回答](https://vue3js.cn/interview/vue/show_if.html#%E4%B8%89%E3%80%81v-show%E4%B8%8Ev-if%E5%8E%9F%E7%90%86%E5%88%86%E6%9E%90)


- 控制手段：v-show隐藏则是为该元素添加css--display:none，dom元素依旧还在。v-if显示隐藏是将dom元素整个添加或删除

- 编译过程：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换

- 编译条件：v-if是真正的条件渲染，它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。只有渲染条件为假时，并不做操作，直到为真才渲染

- 在组件身上使用 v-show 由false变为true的时候不会触发组件的生命周期

- 在组件身上使用 v-if 由false变为true的时候，触发组件的beforeCreate、create、beforeMount、mounted钩子，由true变为false的时候触发组件的beforeDestory、destoryed方法

- 性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗；



v-if 是根据条件渲染出来相应的 DOM，而 v-show 是将 DOM 生成出来，然后将不满足条件的 display 设为 none

场景：
v-if 用于不需要频繁切换的场景，因为首次会按条件生成 DOM，性能比较好
v-show 用于频繁切换的场景，因为设置元素显示与隐藏比 DOM 生成又销毁性能更好

附加：讲真的 v-if 才是真正意义上的`条件渲染`,在渲染函数中它会根据 判断条件 生成三元表达式，而 v-show 则不会