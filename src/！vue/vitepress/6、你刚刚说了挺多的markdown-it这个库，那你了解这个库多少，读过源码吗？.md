### 你刚刚说了挺多的 markdown-it 这个库，那你对这个库了解多少，读过源码吗？

[官方文档](https://markdown-it.github.io/markdown-it)
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



### 如何对 markdown-it 的一些规则进行扩展呢？（怎么编写 plugin 呢？）！！！很重要噻
[编写插件一，总览写 markdown-it 中的 plugin 时的常规四种写法](https://juejin.cn/post/7055238938092371975)
[编写插件二， 从 parse 下手](https://juejin.cn/post/7055597191150174238)
[编写插件三，从 render 下手](https://juejin.cn/post/7056705090811330574)

总结一下，写 plugin 可以从以下几点下手：
1、在 markdown-it 的结果外面包一层代码（标签）
2、对 markdown-it 的结果进行替换，通过 replace API 和 正则更改一部分结果的内容进行返回
3、对解析后的 token 进行更改，然后添加一些内容（比如：属性）然后让内置的 markdown-it 的原来的 rule 进行渲染生成代码
4、对原来的render 的 rule 进行更改或者扩展，然后增加或者更改内容。（比如内置的 containerPlugin 就是给结果外边增加代码的）
5、（最复杂的）给 markdown-it 的 parse 解析的时候按照`合理的顺序(比如 markdown-it 源码就是先进行 block 的解析再进行 inline 的解析，因为block可能包含inline，这样防止 inline 被忘记解析)`插入自己写的 ruler，然后通过自己定义规则的正则然后进行匹配内容，然后将想要的效果变成 token 插入（或替换）到原先的 token 里面，然后再 render 的 ruler 进行 render 即可。例子：[编写插件二，从 parse 下手](https://juejin.cn/post/7055597191150174238)
```js
      md.block.ruler.before('paragraph', 'myplugin', function (state: any, startLine: any, endLine: any) {
        var ch, level, tmp, token,
          // 每一行的开始位置
          pos = state.bMarks[startLine] + state.tShift[startLine],
          // 每一行的结束位置 
          max = state.eMarks[startLine];
        ch = state.src.charCodeAt(pos);

        // 如果开始位置大于等于结束位置 返回 false 就不继续往后遍历执行该函数了
        if (ch !== 0x40/*@*/ || pos >= max) { return false; }

        // 截取该行的内容
        let text = state.src.substring(pos, max);
        // 定义正则
        let rg = /^@\s(.*)/;
        let match = text.match(rg);

        if (match && match.length) {
          // 改变 token 
          let result = match[1];
          token = state.push('heading_open', 'h1', 1);
          token.markup = '@';
          token.map = [startLine, state.line];

          token = state.push('inline', '', 0);
          token.content = result;
          token.map = [startLine, state.line];
          token.children = [];

          token = state.push('heading_close', 'h1', -1);
          token.markup = '@';

          // 返回 true 告诉程序 行 没有遍历完,继续进行遍历 下一行
          state.line = startLine + 1;
          return true;
        }
      })
```