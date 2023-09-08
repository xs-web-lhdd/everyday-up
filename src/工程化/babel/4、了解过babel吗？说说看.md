### 了解过 babel 吗？说说看

#### 为什么需要babel：
在开发中想要使用 ES6+ 的语法，想要使用 TS，JSX 的语法都是离不开 Babel 的

#### Babel 是什么？
Babel 是一个工具链，主要用于旧浏览器或者缓解中将 ES6+ 代码转换为向后版本兼容的 js，babel 采用微内核架构，只编写核心代码像 `@babel/core`, 如果需要babel 完成一些事情就需要编写一些插件，当然官方也会编写很多插件。

包括：语法转换、源代码转换、Polyfill 实现目标缓解缺少的功能等。

babel 是一个独立的工具，不需要必须和其他工具一起使用，可以单独使用

#### 如何使用？
1、首先安装核心 `@babel/core`, 如果需要在命令行使用就安装 `@babel/cli`
```bash
# 将箭头函数转化为普通函数：
npx babel src --out-dir dist --plugins=@babel/plugin-transform-arrow-functions
```
插件的集合：`@babel/preset-env` 这样就可以编译所有 ES6+ 的代码转换为 ES5 的代码
```bash
npx babel src --out-dir dist --presets=@babel/preset-env
```
以上也证明了 `babel` 是一个独立的工具，不用必须和一些其他工具绑定使用


#### Babel 的底层原理：
事实上我们可以将 babel 看成就是一个编译器，编译器的作用就是将我们的源代码，转换成浏览器可以直接识别的另一段源代码。

babel 粗略工作流程：
1、解析阶段（Parsing）
2、转换阶段（Transformation）
3、生成阶段（code Generation）

例子：
ES6+ 代码转换为 ES5 的代码过程：
                 进行更改AST  代码生成器
ES6 --->>> 原AST --->>> 新AST --->>> ES5


babel 详细编译器执行过程：
原生源代码 -------> 词法分析 ------> tokens数组 ------> 语法分析 ------> AST ------> 遍历 ------> 访问 ------> 应用插件 ------> 新的AST ------> 目标源代码

词法分析：
将代码分析成一个一个的 token ，里面类型有 keyword（关键字）、Identifier（标识符）、Punctuator（运算符）

插件：
一般作用于访问AST节点的时候把对应的节点内容进行更改，从而形成新的 AST 语法树


#### Babel 配置文件：
除了在命令行里面输入命令，还可以将 Babel 的配置信息单独写一个独立的文件中，babel给我们提供两种配置文件的编写：
1、.babelrc.json（或者 .babelrc .js .cjs .mjs） 早期使用较多的配置方式，但是对于配置 Monorepos 项目是比较麻烦的。
2、babel.config.json（babel7）（或者 .js .cjs .mjs） 可以直接作用域 Monorepos 项目的子包，更加推荐。

#### Polyfill：
比如我们使用了一些语法特性（例如：Promise、Generator、Symbol 等以及实例方式例如：Array.prototype.includes等）
但是某些浏览器压根不认识这些特性，必然会报错
我们可以使用polyfill来填充或者说打一个补丁，那么就会包含该特性了

安装依赖：
```bash
npm i core-js regenerator-runtime --save
```
然后配置文件里面有个 `useBuiltIns` 这个属性，三个属性：`false`（不使用polyfill）、`usage`（根据源代码需要哪些polyfill就引入相应的polyfill）、`entry`（只要是目标浏览器需要的polyfill不管源代码需不需要都引入）

> 注意
1、在使用 `usage` 的时候记得在 `webpack.config.js` 里面的 rule 里面使用 `babel-loader` 排除 node_modules ，因为可能使用的某些库也用到了 polyfill，这样可能会出现库里面的polyfill和`usage`引入的polyfill版本啥子的不对应从而造成冲突出现报错。
2、直接使用 `entry` 是不生效的，需要在`入口`文件中引入 core.js 和 regenerator-time 才有效，才会根据目标浏览引入所有对应的polyfill； 例如： `import core.js; import regenerator-runtime/runtime;` 即可。

#### 认识 Plugin-transform-runtime
```bash
npm i @babel/plugin-transform-runtime --save
```
上面使用的polyfill默认情况下是添加的所有特性都是全局的
  - 如果我们正在编写一个工具库，这个工具库需要使用 polyfill
  - 别人在使用我们的工具时，工具库通过polyfill添加的特性，可能会污染它们的代码
  - 所以当编写工具时，babel更推荐我们使用一个插件，`@babel/plugin-transform-runtime`来完成polyfill的功能

#### 区分 useBuiltIns 和 Plugin-transform-runtime？
useBuiltIns 把 polyfill 是添加到全局作用域的
Plugin-transform-runtime 是不会添加到全局作用域中的，所以这个不会污染环境的（一般用于第三方库）