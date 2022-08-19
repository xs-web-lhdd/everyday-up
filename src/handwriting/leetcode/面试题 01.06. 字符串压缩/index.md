##### 面试题 01.06. 字符串压缩:
```js
/**
 * @param {string} s
 * @return {string}
 */
var compressString = function (s) {
  let arr = []
  let ans = ''
  for (let i = 0; i < s.length; i++) {
    if (!arr.length || arr[arr.length - 1] === s[i]) arr.push(s[i])
    else {
      ans += `${arr[arr.length - 1]}${arr.length}`
      arr.length = 0
      arr.push(s[i])
    }
  }

  ans += `${arr[arr.length - 1]}${arr.length}`

  return ans.length < s.length ? ans : s
};
```