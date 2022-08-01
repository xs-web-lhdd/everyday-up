##### js中的鼠标事件有哪些？

`DOM3`级事件中定义了9个鼠标事件。

1. `mousedown`
2. `mouseup`
3. `click`
4. `dblclick`
5. `mouseover`
6. `mouseout`
7. `mouseenter`
8. `mouseleave`
9. `mousemove`

-  页面中的所有元素都支持鼠标事件。除了`mouseenter`和`mouseleave`，所有鼠标事件都会冒泡，都可以被取消。

可以对9种鼠标事件分为三类：

- `mousedown`，`mousemove`，`mouseup`: 按下，移动，释放鼠标。
- `click`，`dblclick`: 单击，双击鼠标。
- `mouseover`，`mouseout`：鼠标从一个元素上移入/移出（冒泡）。
- `mouseenter`，`mouseleave`: 鼠标从一个元素上移入/移出（不冒泡）。

https://juejin.cn/post/6923929057994211336

