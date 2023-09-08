### 15、知道 keep-alive 吗？详细说说？

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


### keep-alive 怎么实现的？


### 对比一下 v-show、v-if 和 keep-alive 的切换渲染的不同？
组件如果用 v-if 进行控制，如果频繁的进行切换（组件的挂载和卸载是一个递归的过程）会有一定的性能损耗，为此在这种场景下使用 `keep-alive` 就非常合适。

keep-alive 是一个抽象组件，并不会生成一个真实的 dom，只会渲染内部包裹的子节点,渲染的是它的第一个子节点，并让内部的子节点在切换的时候不会走一整套递归卸载、挂载DOM的流程，从而优化了性能。

keep-alive 使用了两个钩子函数：`onMounted` `onUpdated`



1、组件的渲染：
  使用 composition API 的方式，setup 返回一个函数，就是组件的渲染函数，首先会从keep-alive内部的slot里面拿到内部组件，因为长度只能为1，所以当孩子的长度大于1就会报警告，这也说明了keep-alive是一个抽象组件，不会渲染任何dom，只会渲染内部包裹的子节点。
2、缓存的设计：
  首先组件的递归 patch 过程主要就是为了渲染dom而这个递归过程会有性能消耗，既然目标是dom，那么我们直接把dom缓存了这样下次使用时直接从缓存中去拿，就不需要再递归渲染去耗费性能了。keep-alive 就是这样做的，它注入了两个钩子函数 `onMounted` `onUpdated` 在这两个钩子函数里面进行缓存VDOM，因为给keep-alive组件打上了标记（shapeFlag），所以在更新的时候会执行上下文中的active函数（拿到缓存的vnode然后进行 patch 挂载，接着执行activated钩子函数即可）
3、props 的设计：
  keep-alive支持三个props：include、exclude、max，如果子组件 不匹配include和匹配exclude 那么就不缓存，另外对include和exclude进行了响应式处理（通过watch进行监听），如果其发生变化，删除哪些不匹配的节点即可。因为内存要进行合理使用，不能无限制的进行缓存，所以可以通过 max 限制缓存的个数，当缓存新的 vnode 的时候，做一定程度的缓存管理（这里采用了LRU的思想，删除最久的不用的vnode）
4、组件的卸载：
  组件在卸载的时候如何发现是keep-alive组件就会执行keep-alive组件上下文中的 `deactivate` 函数，这个函数首先从DOM树中移除该节点（注意这里只是移除了DOM并没有执行真正意义上执行子组件的卸载流程）又因为卸载的递归特性会执行keep-alive的卸载，这时就会执行 `onBeforeUnmount` 钩子函数，它会比对所有的 vnode，如果是keep-alive缓存的 vnode，会移除shapeFlag标记，这样就不是keep-alive组件了，就会走正常的卸载逻辑，然后执行子组件的 `deactivated` 钩子函数，如果不是就直接执行 unmount 方法即可。
