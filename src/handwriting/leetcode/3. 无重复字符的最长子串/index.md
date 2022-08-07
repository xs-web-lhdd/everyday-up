##### 3.无重复字符的最长字串

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    var ans = 0
    var arr = []
    for(var i = 0; i < s.length; i++) {
        if(arr.includes(s[i])) {
            var index = arr.indexOf(s[i])
            arr = arr.slice(index + 1)
        } 
        arr.push(s[i])
        ans = Math.max(ans, arr.length)
    }

    return ans
};
```

