### 4、watch 和 watchEffect 有什么区别？

#### 体验
- watchEffect立即运行一个函数，然后被动地追踪它的依赖，当这些依赖改变时重新执行该函数。
Runs a function immediately while reactively tracking its dependencies and re-runs it whenever the dependencies are changed.
```JS
const count = ref(0)

watchEffect(() => console.log(count.value))
// -> logs 0

count.value++
// -> logs 1
```

- watch侦测一个或多个响应式数据源并在数据源变化时调用一个回调函数。
Watches one or more reactive data sources and invokes a callback function when the sources change.
```JS
const state = reactive({ count: 0 })
watch(
  () => state.count,
  (count, prevCount) => {
    /* ... */
  }
)
```

#### 思路:
- 给出两者定义
- 给出场景上的不同
- 给出使用方式和细节
- 原理阐述


#### 范例:
- watchEffect立即运行一个函数，然后被动地追踪它的依赖，当这些依赖改变时重新执行该函数。watch侦测一个或多个响应式数据源并在数据源变化时调用一个回调函数。

- watchEffect(effect)是一种特殊watch，传入的函数既是依赖收集的数据源，也是回调函数。如果我们`不关心响应式数据变化前后的值，只是想拿这些数据做些事情`，那么`watchEffect`就是我们需要的。watch更底层，可以接收多种数据源，包括用于依赖收集的getter函数，因此它完全可以实现watchEffect的功能，同时由于可以指定getter函数，依赖可以控制的更精确，还能获取数据变化前后的值，因此如果需要这些时我们会使用watch。

- watchEffect在使用时，传入的函数会立刻执行一次。watch默认情况下并不会执行回调函数，除非我们手动设置`immediate`选项。

- 从实现上来说，watchEffect(fn)相当于watch(fn,fn,{immediate:true})


#### 知其所以然
watchEffect定义：https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/apiWatch.ts#L80-L81
```JS
export function watchEffect(
  effect: WatchEffect,
  options?: WatchOptionsBase
): WatchStopHandle {
  return doWatch(effect, null, options)
}
```
watch定义如下：https://github1s.com/vuejs/core/blob/HEAD/packages/runtime-core/src/apiWatch.ts#L158-L159
```JS
export function watch<T = any, Immediate extends Readonly<boolean> = false>(
  source: T | WatchSource<T>,
  cb: any,
  options?: WatchOptions<Immediate>
): WatchStopHandle {
  return doWatch(source as any, cb, options)
}
```
很明显watchEffect就是一种特殊的watch实现。