### 1、说说 $attrs 和 $listeners 的使用场景？

#### 分析
API考察，但\$attrs和$listeners是比较少用的边界知识，而且vue3有变化，$listeners已经移除，还是有细节可说的。

#### 思路
- 这两个api的作用
- 使用场景分析
- 使用方式和细节
- vue3变化


#### 体验：
一个包含组件透传属性的对象。
```js
<template>
	<child-component v-bind="$attrs">
        将非属性特性透传给内部的子组件
    </child-component>
</template>
```

#### 范例
1、我们可能会有一些属性和事件没有在props中定义，这类称为非属性特性，结合v-bind指令可以直接透传给内部的子组件。
2、这类“属性透传”常常用于包装高阶组件时往内部传递属性，常用于爷孙组件之间传参。比如我在扩展A组件时创建了组件B组件，然后在C组件中使用B，此时传递给C的属性中只有props里面声明的属性是给B使用的，其他的都是A需要的，此时就可以利用v-bind="$attrs"透传下去。
3、最常见用法是结合v-bind做展开；$attrs本身不是响应式的，除非访问的属性本身是响应式对象。
4、vue2中使用$listeners获取事件，vue3中已移除，均合并到$attrs中，使用起来更简单了。



#### 原理
查看透传属性foo和普通属性bar，发现vnode结构完全相同，这说明vue3中将分辨两者工作由框架完成而非用户指定：
```html
<!-- A 组件 -->
<template>
  <h1>{{ msg }}</h1>
  <comp foo="foo" bar="bar" />
</template>
```
```html
<!-- comp 组件 -->
<template>
  <div>
    {{$attrs.foo}} {{bar}}
  </div>
</template>
<script setup>
defineProps({
  bar: String
})
</script>
```
编译后的渲染函数：
```js
_createVNode(Comp, {
    foo: "foo",
    bar: "bar"
})
```