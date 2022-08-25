##### 15、知道 keep-alive 吗？详细说说？

###### 思路：
1、缓存用 keep-alive，它的作用和用法
2、使用细节，例如缓存 指定/排除、结合 router 和 transition（尤其是在 vue3 中）
3、组件缓存后更新可以利用 activated 或者 beforeRouteEnter
4、原理阐述


###### 回答范例：
开发中缓存组件使用keep-alive组件，keep-alive是vue内置组件，keep-alive包裹动态组件component时，会缓存不活动的组件实例，而不是销毁它们，这样在组件切换过程中将状态保留在内存中，防止重复渲染DOM。
```html
<keep-alive>
  <component :is="view"></component>
</keep-alive>
```
结合属性include和exclude可以明确指定缓存哪些组件或排除缓存指定组件。vue3中结合vue-router时变化较大，之前是keep-alive包裹router-view，现在需要反过来用router-view包裹keep-alive：
```html
<router-view v-slot="{ Component }">
  <keep-alive>
    <component :is="Component"></component>
  </keep-alive>
</router-view>
```
缓存后如果要获取数据，解决方案可以有以下两种：

- beforeRouteEnter：在有vue-router的项目，每次进入路由的时候，都会执行beforeRouteEnter
```js
beforeRouteEnter(to, from, next){
  next(vm=>{
    console.log(vm)
    // 每次进入路由执行
    vm.getData()  // 获取数据
  })
},
```
- actived：在keep-alive缓存的组件被激活的时候，都会执行actived钩子
```js
activated(){
	  this.getData() // 获取数据
},
```
keep-alive是一个通用组件，它内部定义了一个map，缓存创建过的组件实例，它返回的渲染函数内部会查找内嵌的component组件对应组件的 `vnode`，如果该组件在map中存在就直接返回它。由于component的is属性是个响应式数据，因此只要它变化，keep-alive的render函数就会重新执行。
知其所以然
KeepAlive定义

https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/components/KeepAlive.ts#L73-L74

缓存定义

https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/components/KeepAlive.ts#L102-L103

缓存组件

https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/components/KeepAlive.ts#L215-L216

获取缓存组件

https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/components/KeepAlive.ts#L241-L242

测试缓存特性，test-v3.html