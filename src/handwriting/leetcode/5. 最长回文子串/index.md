##### 5. 最长回文子串
###### 双指针法:
```js
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    if(s.length <= 1) return s
    let maxStr = ''
    for(let i = 0; i < s.length; i++) {
        // 奇数情况:
        getStr(i, i)
        // 偶数情况:
        getStr(i, i + 1)
    }

    function getStr(l, r) {
        while(l >= 0 && r < s.length && s[l] === s[r]) {
            l--
            r++
        }
        if(maxStr.length < r - l) {
            maxStr = s.slice(l + 1, r)
        }
    }

    return maxStr
};
```