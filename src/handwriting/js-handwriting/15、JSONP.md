##### 手写 JSONP：
[跨域](https://juejin.cn/post/6844903767226351623#heading-1)
```js
function JSONP({
  url,
  params,
  callBack
}) {
  return new Promise(resolve => {
    const script = document.createElement('script')
    window[callBack] = function (data) {
      resolve(data)
      document.body.removeChild(script)
    }
    params = {
      ...params,
      callBack
    }
    let arr = []
    for (let key in params) {
      arr.push(`${key}=${params[key]}`)
    }
    script.src = `${url}?${arr.join('&')}`
    document.body.appendChild(script)
  })
}
```