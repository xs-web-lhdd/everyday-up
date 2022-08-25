### 28、template 到 render 的过程？
#### 分析
问我们template到render过程，其实是问vue编译器工作原理。

#### 思路
引入vue编译器概念
说明编译器的必要性
阐述编译器工作流程

#### 回答范例
- Vue中有个独特的编译器模块，称为“compiler”，它的主要作用是将用户编写的template编译为js中可执行的render函数。
- 之所以需要这个编译过程是为了便于前端程序员能高效的编写视图模板。相比而言，我们还是更愿意用HTML来编写视图，直观且高效。手写render函数不仅效率底下，而且失去了编译期的优化能力。
- 在Vue中编译器会先对template进行解析，这一步称为parse，结束之后会得到一个JS对象，我们成为抽象语法树AST，然后是对AST进行深加工的转换过程，这一步成为transform，最后将前面得到的AST生成为JS代码，也就是render函数。

#### 知其所以然
vue3编译过程窥探：
https://github1s.com/vuejs/core/blob/HEAD/packages/compiler-core/src/compile.ts#L61-L62

测试，test-v3.html

#### 可能的追问
- Vue中编译器何时执行？（随着引入的运行时是不同的）
  - webpack 环境，使用的就是预打包环境，（webpack 的 vue-loader 会提前将那些模板编译了，这个时候编译就在打包阶段）
  - 非运行时版本，（携带编译器的 vue），这时候工作会在运行时，当发现一个组件的时候，发现这个组件没有渲染函数，用户写了一个模板，这时就会去编译模板，执行时刻会在组件的创建阶段，会在运行时的阶段。
- react有没有编译器？
react 用的 jsx，用的不是编译器，严格来说是转义器（transpiler：语言本身没有发生变化，让 js 变成 js，这严格来说 react 在描述视图的时候是没有编译器的）