### 30、了解那些vue性能优化？

#### 分析
这是一道综合实践题目，写过一定数量的代码之后小伙伴们自然会开始关注一些优化方法，答得越多肯定实践经验也越丰富，是很好的题目。

#### 答题思路：
根据题目描述，这里主要探讨Vue代码层面的优化

#### 回答范例
我这里主要从Vue代码编写层面说一些优化手段，例如：代码分割、服务端渲染、组件缓存、长列表优化等

- 最常见的路由懒加载：有效拆分App尺寸，访问时才异步加载
```js
const router = createRouter({
  routes: [
    // 借助webpack的import()实现异步组件
    { path: '/foo', component: () => import('./Foo.vue') }
  ]
})
```

- keep-alive缓存页面：避免重复创建组件实例（内存换时间）更好用户体验，且能保留缓存组件状态，比如滚动条之前在哪，回来之后还是在哪
```js
<router-view v-slot="{ Component }">
	<keep-alive>
  	<component :is="Component"></component>
  </keep-alive>
</router-view>
```

- 使用v-show复用DOM：避免重复创建组件，达到隐藏或显示，而不是删除再创建这样会快的多
```js
<template>
  <div class="cell">
    <!-- 这种情况用v-show复用DOM，比v-if效果好 -->
    <div v-show="value" class="on">
      <Heavy :n="10000"/>
    </div>
    <section v-show="!value" class="off">
      <Heavy :n="10000"/>
    </section>
  </div>
</template>
```

- v-for 遍历避免同时使用 v-if：实际上在Vue3中已经是个错误写法
```js
<template>
    <ul>
      <li
        v-for="user in activeUsers"
        <!-- 避免同时使用，vue3中会报错 -->
        <!-- v-if="user.isActive" -->
        :key="user.id">
        {{ user.name }}
      </li>
    </ul>
</template>
<script>
  export default {
    computed: {
      activeUsers: function () {
        return this.users.filter(user => user.isActive)
      }
    }
  }
</script>
```

- v-once和v-memo：不再变化的数据使用v-once
v-once 只渲染一次
```html
<!-- single element -->
<span v-once>This will never change: {{msg}}</span>
<!-- the element have children -->
<div v-once>
  <h1>comment</h1>
  <p>{{msg}}</p>
</div>
<!-- component -->
<my-component v-once :comment="msg"></my-component>
<!-- `v-for` directive -->
<ul>
  <li v-for="i in list" v-once>{{i}}</li>
</ul>
```
按条件跳过更新时使用v-memo：下面这个列表只会更新选中状态变化项
```html
<div v-for="item in list" :key="item.id" v-memo="[item.id === selected]">
  <p>ID: {{ item.id }} - selected: {{ item.id === selected }}</p>
  <p>...more child nodes</p>
</div>
```
https://vuejs.org/api/built-in-directives.html#v-memo

- 长列表性能优化：如果是大数据长列表，可采用虚拟滚动，只渲染少部分区域的内容
```js
<recycle-scroller
  class="items"
  :items="items"
  :item-size="24"
>
  <template v-slot="{ item }">
    <FetchItemView
      :item="item"
      @vote="voteItem(item)"
    />
  </template>
</recycle-scroller>
```
一些开源库：
vue-virtual-scroller
vue-virtual-scroll-grid


- 事件的销毁：Vue 组件销毁时，会自动解绑它的全部指令及事件监听器，但是仅限于组件本身的事件。
```js
export default {
  created() {
    this.timer = setInterval(this.refresh, 2000)
  },
  beforeUnmount() {
    clearInterval(this.timer)
  }
}
```

- 图片懒加载:
对于图片过多的页面，为了加速页面加载速度，所以很多时候我们需要将页面内未出现在可视区域内的图片先不做加载， 等到滚动到可视区域后再去加载。
```html
<img v-lazy="/static/img/1.png">
```
参考项目：vue-lazyload

- 第三方插件按需引入:
像element-plus这样的第三方组件库可以按需引入避免体积太大。
```js
import { createApp } from 'vue';
import { Button, Select } from 'element-plus';

const app = createApp()
app.use(Button)
app.use(Select)
```

- 子组件分割策略：较重的状态组件适合拆分
```html
<template>
  <div>
    <ChildComp/>
  </div>
</template>

<script>
export default {
  components: {
    ChildComp: {
      methods: {
        heavy () { /* 耗时任务 */ }
      },
      render (h) {
        return h('div', this.heavy())
      }
    }
  }
}
</script>
```
但同时也不宜过度拆分组件，尤其是为了所谓组件抽象将一些不需要渲染的组件特意抽出来，组件实例消耗远大于纯dom节点。参考：https://vuejs.org/guide/best-practices/performance.html#avoid-unnecessary-component-abstractions


- 服务端渲染/静态网站生成：SSR/SSG
如果SPA应用有首屏渲染慢的问题，可以考虑SSR、SSG方案优化。参考SSR Guide




### 说说 vue 长列表优化思路：
#### 回答
1、在大型企业级项目中经常需要渲染大量数据，此时很容易出现卡顿的情况。比如大数据量的表格、树。

2、处理时要根据情况做不通处理：
- 避免大数据量：可以采取分页的方式获取，避免渲染大量数据
- 避免渲染大量数据：`vue-virtual-scroller`等虚拟滚动方案，只渲染视口范围内的数据
- 避免更新：如果不需要更新，可以使用v-once方式只渲染一次
- 优化更新：通过v-memo可以缓存结果，结合v-for使用，避免数据变化时不必要的VNode创建
- 按需加载数据：可以采用懒加载方式，在用户需要的时候再加载数据，比如tree组件子树的懒加载

3、总之，还是要看具体需求，首先从`设计上避免大数据获取和渲染`；实在需要这样做可以采用`虚表的方式优化渲染`；最后`优化更新`，如果不需要更新可以v-once处理，需要更新可以v-memo进一步优化大数据更新性能。其他可以采用的是`交互方式优化`，无线滚动、懒加载等方案。