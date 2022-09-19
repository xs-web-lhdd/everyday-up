### 38、mixin 和 data 的优先级？
1.对于created，mounted 等生命周期函数 mixin文件中的代码先执行，组件中的后执行
2.对于data中定义的字段，组件中定义组件覆盖mixin中同名字段
3.对于 method中的同名方法，组件内的同名方法覆盖mixin中的方法