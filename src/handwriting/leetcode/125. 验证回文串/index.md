##### 135. 验证回文串：
```js
/**
 * @param {string} s
 * @return {boolean}
 */
 var isPalindrome = function(s) {
  s = s.toLowerCase()

  let str = ''
  for(var i = 0; i < s.length; i++) {
      if(s[i].charCodeAt() >= 65 && s[i].charCodeAt() <= 90 || s[i].charCodeAt() >= 97 && s[i].charCodeAt() <= 122 || s[i].charCodeAt() >= 48 && s[i].charCodeAt() <= 57 ) {
          str += s[i]
      }
  }

  if(str.length % 2) {
    // 是奇数的情况：
    var mid = Math.floor(str.length / 2)
    var s1 = str.slice(0, mid)
    var s2 = str.slice(mid+1)
    return s1.split('').reverse().join('') == s2
  } else {
    // 是偶数的情况：
    var mid = str.length / 2
    var s1 = str.slice(0, mid)
    var s2 = str.slice(mid)
    return s1.split('').reverse().join('') == s2
  }
};
```