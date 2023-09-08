### 56、ajax的发送流程？
1、创建 XMLHttpRequest 对象：
```js
let xhr = new XMLHttpRequest()
```

2、向服务器发送请求：
```js
xhr.open(method,url,async);
xhr.send();
```

open 参数：
1、method 请求的类型，GET 或者 POST
2、url 文件在服务器上的位置
3、async：true异步 false同步

async 代表异步，即 脚本会在 send() 方法之后继续执行，而不等待来自服务器的响应

只有请求方法是 post 的时候，才使用 send() 里面发送参数


3、异步处理，添加 `onreadystatechange` 监听函数：
```js
 xmlhttp.onreadystatechange=function () {//接收到服务端响应时触发
      if(xmlhttp.readyState==4&&xmlhttp.status==200){
            document.getElementById("mydiv").innerHTML=xmlhttp.responseText;
       }
 }
```
`readyState` 一共有5中请求状态，从0 到 4 发生变化。
0: 请求未初始化
1: 服务器连接已建立
2: 请求已接收
3: 请求处理中
4: 请求已完成，且响应已就绪

`status` 表示状态码