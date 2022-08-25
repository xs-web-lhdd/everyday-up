### 如果让你从 0 写一个 vuex 库，情说说你的思路？

#### 回答范例
官方说vuex是一个状态管理模式和库，并确保这些状态以可预期的方式变更。可见要实现一个vuex：
- 要实现一个Store存储全局状态
- 要提供修改状态所需API：commit(type, payload), dispatch(type, payload)

1、实现Store时，可以定义Store类，构造函数接收选项options，设置属性state对外暴露状态，提供commit和dispatch修改属性state。这里需要设置state为响应式对象，同时将Store定义为一个Vue插件。
2、commit(type, payload)方法中可以获取用户传入mutations并执行它，这样可以按用户提供的方法修改状态。dispatch(type, payload)类似，但需要注意它可能是异步的，需要返回一个Promise给用户以处理异步结果。