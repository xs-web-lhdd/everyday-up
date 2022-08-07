##### link和@import引入css的区别？

```bash
(1) link属于HTML标签，而@import是CSS提供的;

(2) 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;

(3) import只在IE5以上才能识别，而link是HTML标签，无兼容问题;

(4) link可以动态添加样式，属于html标签，可以通过dom进行添加，而@import不可以
```