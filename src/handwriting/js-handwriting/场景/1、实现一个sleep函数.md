##### 1、实现一个 sleep 函数：
```js
async function a() {
  for (let i = 0; i < 10; i++) {
    await Promise.sleep(1);
    console.log(i)
  }
}

Promise.sleep = function (timeout) {
  return new Promise((resolve) => {
    setTimeout(resolve, timeout * 1000)
  })
}

a()
```