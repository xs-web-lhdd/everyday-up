### 你刚刚说了挺多的 markdown-it 这个库，那你对这个库了解多少，读过源码吗？

> 大佬文章：https://juejin.cn/post/7054876950589866021

是什么：`markdown-it`是一个 markdown 解析器，并且易于拓展。
有什么好处：
- 可配置语法，你可以添加新的规则或者替换掉现有的规则
- 快
- 社区有很多的插件或者其他包


`markdown-it` 主要工作过程：
1、Parse：将 Markdown 文件 Parse 为 Tokens
2、Render：遍历 Tokens 生成 HTML

> 跟 Babel 很像，不过 Babel 是转换为抽象语法树（AST），而 markdown-it 没有选择使用 AST，主要是为了遵循 KISS(Keep It Simple, Stupid) 原则。

`markdown-it` 的核心过程就是将 markdown 的内容解析为 tokens，然后遍历 tokens 然后结合 渲染的规则 将其渲染为 html。在这个过程中 parse 和 render 都有一些规则，这里的 parse 以  `lib/parse_core.js` 为例：
```js
// parse 过程中的默认有 6 条规则
var _rules = [
  // 使用 normalize.css 抹平各端的差异：
  ['normalize', require('./rules_core/normalize')],
  // block 先执行吗，因为 block 里面包含 inline，但是 inline 不会包含 block
  ['block', require('./rules_core/block')],
  ['inline', require('./rules_core/inline')],
  // linkify 自动识别链接
  ['linkify', require('./rules_core/linkify')],
  // 将 (c)`` (C) 替换成 ©，将 ???????? 替换成 ???，将 !!!!! 替换成 !!!，诸如此类：
  ['replacements', require('./rules_core/replacements')],
  // 为了方便印刷，对直引号做了处理：
  ['smartquotes', require('./rules_core/smartquotes')],
  // `text_join` finds `text_special` tokens (for escape sequences)
  // and joins them with the rest of the text
  ['text_join', require('./rules_core/text_join')]
];
```
在 `lib/renderer.js` 中是默认的渲染规则：
```js
default_rules.code_inline
default_rules.code_block
default_rules.fence
default_rules.image
default_rules.hardbreak
default_rules.softbreak
default_rules.text
default_rules.html_block
default_rules.html_inline
```
这里以 `default_rules.code_inline` 为例:
```js
// 源代码：
default_rules.code_inline = function (tokens, idx, options, env, slf) {
  var token = tokens[idx];

  return '<code' + slf.renderAttrs(token) + '>' +
    escapeHtml(token.content) +
    '</code>';
};
```
其实很简单，就是将 token中的 content 也就是 解析出来的内容通过 `code` 标签包一层，然后如果有 attrs 就给 code 标签加上 对应的 attr 即可 



### 如何对 markdown-it 的一些规则进行扩展呢？（怎么编写 plugin 呢？）
[编写插件一](https://juejin.cn/post/7055238938092371975)
[编写插件二](https://juejin.cn/post/7055597191150174238)
[编写插件三](https://juejin.cn/post/7056705090811330574)