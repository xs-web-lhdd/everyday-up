##### 12、如何组织 form 表单提交？
1. `<button>` 提交时，type默认为submit，阻止提交将type改为button，即 `type="button"`
2. `<input>` 提交时，将type改为button，即 `type="button"`
3. 在提交按钮上绑定点击事件，使用 preventDefault() 方法
4. 在提交按钮上，使用 `return false;`