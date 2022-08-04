##### 写个方法判断当前脚本运行在浏览器还是 node 环境中。(2020.01.14)

> 分析：类似上一个题，区分 `window.console.log` 与 `console.log`，主要是在于区分当前全局变量是 `window` 还是 `global`

- 解答：

  - 初版：判断 `window` 是否存在，并且为对象即可。

  ```js
  const isBrowser = typeof window === "object" && window;
  const isNode = typeof global === "object" && global;
  ```

  > 上面的代码虽然看起来实现了功能，但是有一个很致命的瑕疵，如果 `browser` 中 `global = window`，那就无法完成区分。

  - 参考版：除上述版本，还查到类似判断 `this === window` 来进行判断，但是瑕疵与上面相同。

  - 完善版：`[参考 Gloomysunday28]`

    > `js` 有一个全局的 `self` 对象，`self === window, self.self === window`

    ```js
    const isBrowser = typeof self === "object" && self.self === self && self;
    const isNode =
      typeof global === "object" && global.global === global && global;
    ```

