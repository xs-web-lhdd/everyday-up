##### 手写 JSONP：
[跨域](https://juejin.cn/post/6844903767226351623#heading-1)
```js
// 写法一：
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

```js
// 写法二：
function JsonP({
  url,
  params,
  callback
}) {
  return new Promise((resolve) => {
    function getUrl() {
      let data = ''
      for (let key in params) {
        if (params.hasOwnProperty(key)) {
          data += `${key}=${params[key]}&`
        }
      }
      data += `callback=${callback}`

      return `${url}?${data}`
    }
    let script = document.createElement('script')
    script.src = getUrl()
    document.appendChild(script)
    window[callback] = function (data) {
      resolve(data)
      document.removeChild(script)
    }
  })
}
```