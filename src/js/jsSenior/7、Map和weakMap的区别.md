##### map与weakMap的区别？

WeakMap 结构与 Map 结构类似，也是用于生成键值对的集合。

+ 只接受对象作为键名（null 除外），不接受其他类型的值作为键名
+ 键名是弱引用，键值可以是任意的，键名所指向的对象可以被垃圾回收，此时键名是无效的
+ 不能遍历，方法有 get、set、has、delete

##### 为什么weakMap不能遍历？

其实也比较好理解，因为键是弱引用，不知道那一瞬间就被垃圾回收了，所以不能遍历也是正常，否则在遍历过程中键被垃圾回收那岂不是报错了

##### set 与 weakSet 区别

`WeakSet` 结构与 Set 类似，也是不重复的值的集合。

1. `WeakSet` 成员都是数组和类似数组的对象，若调用 `add()` 方法时传入了非数组和类似数组的对象的参数，就会抛出错误。
2. `WeakSet` 成员都是弱引用，可以被垃圾回收机制回收，可以用来保存 `DOM` 节点，不容易造成内存泄漏。
3. `WeakSet` 不可迭代，因此不能被用在 `for-of` 等循环中。
4. `WeakSet` 没有 `size` 属性。


##### Map、weakMap、Set、weakSet 的 API 用法：
Map: 
- size 返回键值对的数目
- clear 从对象中和删除所有键值对
- delete(key) 删除某一个键值对
- get(key) 获取 key 对应的值
- has(key) 检查是否有该 key
- set(key, value) 设置键值对
- keys() 返回一个新的 Iterator 对象，该对象包含按插入顺序排列的对象中每个元素的键。
- values() 返回一个新的 Iterator 对象，该对象包含按插入顺序排列的对象中每个元素的值
- entries() 返回一个新的 Iterator 对象，该对象按插入顺序包含对象中每个元素的 [键、值] 数组。
- forEach()

WeakMap:
- delete(key) 删除与 关联的任何值
- get(key) 返回与 关联的值，如果没有，则返回 undefined。
- has(key) 返回布尔值，检验是否有该值
- set(key, value) 设置键值对

Set:
- size
- add(value)
- clear()
- delete()
- has(value)
- values()
- keys()
- enties()
- forEach

WeakSet:
- add()
- delete()
- has()

##### map与object区别？

###### 初始化

1. map 只能通过 new 关键字和构造函数创建
2. object 可以使用字面量、构造函数、Object.crate的形式创建。

###### 键值

1. object键值只能使用数组、字符串、符号作为键
2. map 的键值可以是任意类型

###### 顺序与迭代

1. object的键值 key 的遍历顺序
   + 首先遍历所有数值键，按照数值升序排列。
   + 其次遍历所有字符串键，按照加入时间升序排列。
   + 最后遍历所有 Symbol 键，按照加入时间升序排列。
2. map 会维护键值对的插入顺序，因此遍历顺序就是插入顺序