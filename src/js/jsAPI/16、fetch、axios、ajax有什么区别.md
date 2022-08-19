##### 16、fetch、axios、ajax有什么区别？

这三者根本就不是一个维度的东西，fetch是属于 js 中的内置 API ，是和 XMLHttpRequest 属于一个维度的东西，而 axios 是一个用来通信的请求库，它就是基于 js 的原生 API 进行封装的，但是它使得比原生 API 更加好使，增加 拦截器、支持 Promise 等功能，ajax 是一个技术的统称，指不需要重新刷新页面就能让页面局部刷新的技术统称。

###### 你知道 fetch 的缺点吗？
1、fetch只对网络请求报错，对400，500都当做成功的请求，服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请求不能完成时，fetch 才会被 reject。
2、fetch默认不会带cookie，需要添加配置项： fetch(url, {credentials: 'include'})
3、fetch不支持abort，不支持超时控制，使用setTimeout及Promise.reject的实现的超时控制并不能阻止请求过程继续在后台运行，造成了流量的浪费
