##### 1、手写 call:
```js
    Function.prototype.myCall = function (context, ...arg) {
      // 不传参时就是绑 window
      context = context || window
      const symbolKey = Symbol()
      context[symbolKey] = this
      const res = context[symbolKey](...arg)
      delete context[symbolKey]

      return res
    }
```
测试代码：
```js
    var age = 'Tom'
    const obj = {
      age: 'Jone',
      getAge: function (a, b, c) {
        return this.age + a + b + c
      }
    }

    console.log(obj.getAge());
    console.log(obj.getAge.call(null, 1, 2, 3));
    console.log(obj.getAge.call(window, 1, 2, 3));

    console.log(obj.getAge.myCall(null, 1, 2, 3));
    console.log(obj.getAge.myCall(window, 1, 2, 3));
```