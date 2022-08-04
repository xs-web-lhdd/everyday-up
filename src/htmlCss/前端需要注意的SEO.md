##### 前端需要注意哪些 SEO

> 分析：从 meta 标签使用、前端语义化方面、网站加载速度分析(然而答案比想象中长很多)

- 解答：

  1. `title、meta(keywords/description)`合理使用： - 搜素权限：`title > description > keywords`

  - `title`：强调重点，重要关键词出现不超过两次
  - `description`：内容高度概括，长度合适
  - `keywords`：重要关键词

  2. `HTML` 语义化
  3. 重要内容不要用 `js` 输出：爬虫不会爬取 `js` 执行内容
  4. 非装饰性图片添加 `alt` 属性
  5. 少用 `iframe`：搜索引擎不会抓取 `iframe` 内容
  6. 提高网站速度：搜索引擎排序的一个重要指标
  7. 重要内容 `HTML` 代码放在最前：搜索引擎抓取 `HTML` 顺序是从上到下，而且存在内容上限