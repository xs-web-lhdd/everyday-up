##### Vue的双向绑定原理（发布订阅模式）?



### 能说一说双向绑定使用和原理吗？

##### 回答范例：
1、vue中双向绑定是一个指令v-model，可以绑定一个响应式数据到视图，同时视图中变化能改变该值。

2、v-model是语法糖，默认情况下相当于:value和@input。使用v-model可以减少大量繁琐的事件处理代码，提高开发效率。

3、通常在表单项上使用v-model，还可以在自定义组件上使用，表示某个值的输入和输出控制。

4、通过<input v-model="xxx">的方式将xxx的值绑定到表单元素value上；对于checkbox，可以使用true-value和false-value指定特殊的值，对于radio可以使用value指定特殊的值；对于select可以通过options元素的value设置特殊的值；还可以结合.lazy,.number,.trim对v-mode的行为做进一步限定；v-model用在自定义组件上时又会有很大不同，vue3中它类似于sync修饰符，最终展开的结果是modelValue属性和update:modelValue事件；vue3中我们甚至可以用参数形式指定多个不同的绑定，例如v-model:foo和v-model:bar，非常强大！

4、v-model是一个指令，它的神奇魔法实际上是vue的编译器完成的。我做过测试，包含v-model的模板，转换为渲染函数之后，实际上还是是value属性的绑定以及input事件监听，事件回调函数中会做相应变量更新操作。编译器根据表单元素的不同会展开不同的DOM属性和事件对，比如text类型的input和textarea会展开为value和input事件；checkbox和radio类型的input会展开为checked和change事件；select用value作为属性，用change作为事件。当加入 .lazy 的操作符是 input 中的 input 事件就会变成 change 事件。


##### 可能的追问：
v-model和sync修饰符有什么区别？
  vue3 中相当于 v-model 和 .sync 合起来了
自定义组件使用v-model如果想要改变事件名或者属性名应该怎么做？