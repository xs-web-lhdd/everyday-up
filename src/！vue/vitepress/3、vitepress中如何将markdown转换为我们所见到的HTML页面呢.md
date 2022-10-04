### vitepress 中如何将 markdown 转换为我们所见的 HTML 页面呢？
vitepress 内部使用了 `markdown-it` 这个库，当然不只是单单调用 `markdown-it` 这个库这么简单，还内置了许多自己编写的 `markdown-it` 的 `plugin`（文件位置在 node/markdown/plugins），例如：
`vitepress 的内置插件` **都是对 markdown-it 的 renderder 的 默认 rule 进行一系列扩展，了解 markdown-it 插件如何编写看这些插件的源码就感觉很简单**
```js
// 该文件是用来扩展 markdown-it 的官方插件 markdown-it-container 来实现 vitepress 中的 tip 那种格式的
containerPlugin
//  对内链图片的 url 进行规范化处理，为了是可以找到内部图片资源
imagePlugin
// 这个 pulgin 主要做两件事：
//  1、为外部链接添加 target="_blank"
//  2、为内部链接的末尾添加 .html，因为 md 会转换为 html
linkPlugin
// 显示代码块的行号的 plugin，只要用户在 .vitepress/config.js 的 markdown 中配置了行号选项（lineNumbers），那么就会调用该 plugin
lineNumberPlugin
// 为添加代码块右上角的 copy按钮和代码语言标识
preWrapperPlugin
```
以上的 plugin 是 vitepress 在 markdown-it 的基础上写的内置 plugin，还有一些其他的 plugin，例如：
`mdit-vue 的插件集合(社区的markdown-it插件关于 vue 的)`
```js
// 一个markdown-it插件，允许在 markdown 中使用 Vue 组件:
// 将vue内置组件和未知的 HTML 标签视为 vue 组件（markdown - 默认情况下会将它们视为内联标签）。
// 允许原生 HTML 标签上的vue@指令。
componentPlugin

// 一个markdown-it插件，用于获取 markdown frontmatter 和gray-matter:
// 将 frontmatter 提取到 markdown-itenv.frontmatter中。
// 允许通过 markdown-it 提供默认的 frontmatter env.frontmatter。
// 将没有 frontmatter 的 markdown 内容提取到 markdown-itenv.content中。
// 支持将渲染的摘录提取到 markdown-itenv.excerpt中。
frontmatterPlugin

// 一个markdown-it插件，用于获取 markdown 标头:
// 将所有标题信息提取到 markdown-itenv.headers中。
headersPlugin

// 一个markdown-it插件，帮助将 markdown 转换为Vue SFC:
// 避免渲染<script>和<style>标记并将它们提取到 markdown-itenv.sfcBlocks中。
// 支持提取自定义块。
// 提供env.sfcBlocks.template方便。
sfcPlugin

// 用于获取页面标题的markdown-it插件:
// 将 title（第一个 level-1 标题的内容）提取到 markdown-itenv.title中。
titlePlugin
```
也有 `markdown-it` 的官方插件：
```js
// 表情插件
emojiPlugin
// 为 h 标签添加一个 id 属性变为锚点的插件（将 # 后面的 内容变为 id 属性的值插入到 h 标签中）
// 例如：## Foo --->>> <h2 id="foo">Foo</h2>.
anchorPlugin
// 将大括号里面的内容变为属性添加进去
// 例如：# header {.style-me} paragraph {data-toggle=modal}
// 使用插件后：<h1 class="style-me">header</h1> <p data-toggle="modal">paragraph</p>
attrsPlugin
```

以上扩展的可以通过 node/markdown/markdown.ts 文件即可看到执行顺序