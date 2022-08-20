##### 23、解析 url 参数？
```js
function parseUrl(url) {
  if (!url.includes('?')) return {}
  let params = url.split('?')[1]
  if (!params.includes('&')) {
    let obj = {}
    let [key, value] = params.split('=')
    obj[key] = decodeURIComponent(value)

    return obj
  } else {
    let obj = {}
    let arr = params.split('&')
    arr.forEach(item => {
      let [key, value] = item.split('=')
      obj[key] = decodeURIComponent(value)
    });

    return obj
  }
}


console.log(parseUrl('https: //www.bilibili.com/video/BV11i4y1Q7H2/?spm_id_from=333.788&vd_source=605eaae8506df36fc86fca9b2b498d80'));
```