### 2、router-link 和 router-view 是如何生效的？

#### 回答范例
- vue-router中两个重要组件`router-link`和`router-view`，分别起到`路由导航`作用和`组件内容渲染`作用
- vue-router 会监听 `popstate` 事件，点击 router-link，页面不会刷新，而是拿出当前的 path 和 routes 中 path 匹配，获取到匹配组件后，`router-view`会将其渲染出来
- 使用中router-link默认生成一个a标签，设置to属性定义跳转path。实际上也可以通过custom和插槽自定义最终的展现形式。router-view是要显示组件的占位组件，可以嵌套，对应路由配置的嵌套关系，配合name可以显示具名组件，起到更强的布局作用。
- router-link组件内部根据custom属性判断如何渲染最终生成节点，内部提供导航方法navigate（此方法最终会修改响应式的路由变量，然后重新去routes匹配出数组结果，），用户点击之后实际调用的是该方法，它会`pushState`以激活事件处理函数，重新匹配出一个路由 `injectedRoute`；router-view则根据其所处深度deep在匹配数组结果中找到对应的路由并获取组件，最终将其渲染出来。