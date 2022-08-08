##### 说说你对 getComputedStyle 的理解：

- `getComputedStyle`会获取当前元素所有最终使用的CSS属性值（最终计算后的结果），通过`window.getComputedStyle`等价于`document.defaultView.getComputedStyle`调用
- `window.getComputedStyle(elem, null).getPropertyValue("height")`可能的值为`100px`，而且，就算是css上写的是`inherit`，`getComputedStyle`也会把它最终计算出来的。不过注意，如果元素的背景色透明，那么`getComputedStyle`获取出来的就是透明的这个背景（因为透明本身也是有效的），而不会是父节点的背景。所以它不一定是最终显示的颜色。
- `getComputedStyle`会引起回流，因为它需要获取祖先节点的一些信息进行计算（譬如宽高等），所以用的时候慎用，回流会引起性能问题。