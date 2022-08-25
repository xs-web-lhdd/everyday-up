### 4、怎么监听 vuex 中数据的变化？

#### 回答：
我知道几种方法：
  - 可以通过 `watch` 选项或者 `watch` 方法监听状态
  - 可以使用 vuex 中提供的 API：`store.subscribe()`

- `watch`选项方式，可以以字符串形式监听 `$store.state.xxx`；subscribe 方式，可以调用 `store.subscribe(cb)` 回调函数接受 mutation 对象和 state 对象，这样可以进一步判断 mutation.type 是否是期待的那个，从而进一步做后续处理。

- `watch` 方式，简单好用，且能获取变化前后的值，首选；`subscribe`方法会被所有 commit 行为触发，因从还需要判断 mutation.type，用起来略显繁琐，一般用于 vuex 插件中。


#### 实践：
`watch`方式：
```js
const app = createApp({
  watch: {
    '$store.state.counter'() {
      console.log('发生相应');
    }
  }
})
```

`subscribe`方式：
```js
store.subscribe(mutation, state) {
  if(mutation.type === 'add') {
    console.log('发生相应');
  }
}
```