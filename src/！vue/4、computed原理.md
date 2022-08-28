### 4、computed 原理？


#### 判断第一次渲染的时候计算属性x是否会执行？
```js
new Vue({
    el: '#app',
    data() {
      return {
        a: 1,
        b: 2,
      }
    },
    computed: {
      x: function () {
        return this.a + this.b;
      }
    },
    render: function(createElement) {
      return createElement(
        'h1',
        this.a > 1 ? this.x : this.b
      )
    }
});
```

下面过了2秒后改变a的值，x会发生更新吗？

```js
new Vue({
    el: '#app',
    data() {
      return {
        a: 1,
        b: 2,
      }
    },
    computed: {
      x: function () {
        return this.a + this.b;
      }
    },
    methods: {
      update() {
        setTimeout(() => this.a = 0, 2000);
      }
    },
    mounted() {
      this.update();
    },
    render: function(createElement) {
      return createElement(
        'h1',
        this.a >= 1 ? this.x : this.b
      )
    }
});
```
- 第一个问题的情况，this.x不会执行
 - 初始化computed的时候，会生成computedWatchers，并且将x属性挂载到vm对象上，但由于this.a = 1不满足this.a > 1的条件，不读取this.x则不会触发computed的getter，也就不会触发watcher.evaluate，那么就不会计算了。
- 第二个问题的情况：this.x两次都执行
 - 在初始化的时候，生成了computedWatchers，渲染时满足this.a >= 1使用了this.x，获取this.x时，触发get，计算this.x的结 果，这个过程又需要读取this.a和this.b，分别触发这两个响应式数据的getter并进行依赖收集；
 - 2秒后this.a值改变，更新this.a的依赖computedWatcher以及renderWatcher，因此this.x也会重新计算求值。