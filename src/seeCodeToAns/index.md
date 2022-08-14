### 本目录是前端代码中看代码说输出的题目：

```js
    function f(a) {
      a.x = 1;
      console.log(a.x);
      a = { x: 3 };
      console.log(a.x);
    }
    var a = { x: 0 };
    f(a);
    console.log(a.x);
```
> 知识点：js 的 传参方式
这里面都是按值传参，所以传的是地址，然后 f 里面的 a 开辟的空间的地址与外边 a 的地址一致，但是两者可并不是一个地址，所以 f 内部 a 的栈中地址改变时，外边 a 的栈中地址是不会改变的，所以最后一个 console 结果是 1



```js
async function async1(){
  console.log('async1 start')
  await async2()
  console.log('async1 end')
}
async function async2(){
  console.log('async2')
}
console.log('script start')
setTimeout(function(){
  console.log('setTimeout') 
},0)  
requestAnimationFrame(function(){
    console.log('requestAnimationFrame') 
})
async1();
new Promise(function(resolve){
  console.log('promise1')
  resolve();
}).then(function(){
  console.log('promise2')
})
```
> 微任务和宏任务
涉及 `requestAnimationFrame` 的执行时机
阅读: 
[字节佬讲解](https://zhuanlan.zhihu.com/p/142742003)
[简书一文](https://www.jianshu.com/p/ba5828330aec)