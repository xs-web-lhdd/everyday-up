##### 写一个 obj，使得满足以下程序：

```js
if(obj == 1 && obj == 2 && obj == 3) {
  console.log('我是傻逼');
}
```

解答：

```js
// 第一种：
var obj = {
  value: 1,
  valueOf() {
    return this.value++
  }
}

// 第二种：
var obj = {
  value: 1,
  toString() {
    return this.value++
  }
}

// 第三种：
var obj = {
  value: 1,
  [Symbol.toPrimitive]() {
    return this.value++
  }
}

// 第四种：
var obj = [1,2,3]
obj.join = obj.shift

// 第五种：
var val = 0;
Object.defineProperty(window, 'obj', { // 这里要window，这样的话下面才能直接使用a变量去 ==
    get: function () {
        return ++val;
    }
});

if(obj == 1 && obj == 2 && obj == 3) {
  console.log('我是傻逼');
}
```

