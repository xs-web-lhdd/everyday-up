##### 修改string.length大小能改变字符串长度吗？为什么？:

```js
var str = '123456';
str.length = 0;
console.log(str, str.length); // 123456 6
```

很明显，修改 `str.length` 是无法做到修改字符串的长度的。

`str` 为原始值，调用 `length` 相当借用 `new String(str).length`，修改的是 `new String(str)` 的 `length` ，跟原始值 `str` 无关。

##### 扩展：修改new String()生成字符串的length会生效吗？为什么？

如果将上面代码修改一下，`str` 是由 `new String` 产生，修改 `length` 属性会生效吗？

```js
var str = new String('123456');
str.length = 0;
console.log(str, str.length); // String {"123456"} 6
```

答案告诉我们，还是失败了。

`new String()` 生成的字符串是对象类型，为啥还不能使用 `length` 属性。那说明 `length` 属性，很有可能配置不可写，测试一下上述猜想:

```js
Object.getOwnPropertyDescriptor(str, 'length')
/*
    {
        value: 6, 
        writable: false, 
        enumerable: false, 
        configurable: false
    }
*/
```

由控制台的打印可知：`new String()` 生成的字符串的 `length` 属性不止是不可写，而且是不可枚举、不可配置的。