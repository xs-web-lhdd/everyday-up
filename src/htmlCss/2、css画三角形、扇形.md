##### 一个元素，使用CSS画三角形？画扇形？
```css
/* 三角形：一个div content即可 */
.content {
  width: 0;
  height: 0;
  border: 10px solid transparent;
  border-left: 10px solid red;
}
```
扇形：定位 + transform + 隐藏

```html
<style>
    .content {
      width: 100px;
      height: 100px;
      /* background-color: yellow; */
      position: relative;
      /* 多余的隐藏 */
      overflow: hidden;
    }
    .child {
      width: 200px;
      height: 200px;
      position: absolute;
      background-color: red;
      left: -100px;
      border-radius: 50%;
    }
    #d1 {
      /* 倾斜 */
      transform: skewX(155deg);
      /* 坐标原点 */
      transform-origin: 0 100%;
      position: absolute;
      right: 0;
      top: 0;
      background: #fff;
      width: 100px;
      height: 100px;
    }
  </style>
  <div class="content">
    <div class="child">
      <div id="d1"></div>
    </div>
  </div>
```

