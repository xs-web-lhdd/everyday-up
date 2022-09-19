### 35、vue2和vue3响应式？
[Vue2 和 Vue3 的响应式原理比对---讲的很清楚了赞！最近没看 vue3 ，这篇又帮我回忆起来了！](https://juejin.cn/post/7124351370521477128#heading-3)

#### vue2 中怎么让 data 中的数据失去响应式：
Object.freeze()

#### vue3 中的 ref 是通过 defineProperty 进行拦截的吗？
对也不对！
1、在源码层面很明显 ref 是通过 getter/setter 进行拦截的，在一个 class 里面对属性进行拦截
2、在 babel 转换之后变成了 defineProperty 进行拦截的

#### vue3 中数组虽然也是 Proxy 拦截但却要做很多特殊处理：
[Vue3 中是怎么监测数组的变化？](https://juejin.cn/post/7124351370521477128#heading-6)
- 保证建立正确的依赖关系（比如数组下标修改了，并且下标超过原先数组长度，那么就要更新 length，那么 length 也应该建立对应的依赖关系）
- 一些方法的重写（indexOf、lastIndexOf、includes | 'push','pop','shift', 'unshift', 'splice'）
    ##### indexOf、lastIndexOf、includes：
    - 当数组响应式对象使用 includes、indexOf、lastIndexOf 这方法的时候，它们内部的 this 指向的是代理对象，并且在获取数组元素时得到的值要也是代理对象，所以当使用原始值去数组响应式对象中查找的时候，如果不进行特别的处理，是查找不到的，所以我们需要对上述的数组方法进行重写才能解决这个问题。
    - 首先 arr.indexOf 可以理解为读取响应式对象 arr 的 indexOf 属性，这就会触发 getter 拦截器，在 getter 拦截器内我们就可 以判断 target 是否是数组，如果是数组就看读取的属性是否是我们需要重写的属性，如果是，则使用我们重新之后的方法。
  ```js
    const arrayInstrumentations = {}
    ;(['includes', 'indexOf', 'lastIndexOf']).forEach(key => {
      const originMethod = Array.prototype[key]
      arrayInstrumentations[key] = function(...args) {
        // this 是代理对象，先在代理对象中查找
        let res = originMethod.apply(this, args)

        if(res === false) {
           // 在代理对象中没找到，则去原始数组中查找
           res = originMethod.apply(this.raw, args)
        }
        // 返回最终的值
        return res
      }
    })
  ```
    ##### 'push','pop','shift', 'unshift', 'splice':
    - 进行 arr.push 的操作却也触发了 getter 拦截器，并且触发了两次，其中一次就是数组 push 属性的读取，还有一次是什么呢？还有一次就是调用 push 方法会间接读取 length 属性，那么问题来了，进行了 length 属性的读取，也就会建立 length 的响应依赖，可 arr.push 本意只是修改操作，并不需要建立 length 属性的响应依赖。所以我们需要 “屏蔽” 对 length 属性的读取，从而避免在它与副作用函数之间建立响应联系。
    ```js
      const arrayInstrumentations = {}
      // 是否允许追踪依赖变化
      let shouldTrack = true
      // 重写数组的 push、pop、shift、unshift、splice 方法
      ;['push','pop','shift', 'unshift', 'splice'].forEach(method => {
          // 取得原始的数组原型上的方法
          const originMethod = Array.prototype[method]
          // 重写
          arrayInstrumentations[method] = function(...args) {
              // 在调用原始方法之前，禁止追踪
              shouldTrack = false
              // 调用数组的默认方法
              let res = originMethod.apply(this, args)
              // 在调用原始方法之后，恢复允许进行依赖追踪
              shouldTrack = true
              return res
          }
      })
    ```
    - 在调用数组的默认方法间接读取 length 属性之前，禁止进行依赖跟踪，这样在间接读取 length 属性时，由于是禁止依赖跟踪的状态，所以 length 属性与副作用函数之间不会建立响应联系。
