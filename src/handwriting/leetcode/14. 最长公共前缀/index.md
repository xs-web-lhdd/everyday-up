##### 14. 最长公共前缀:
```js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    // 默认最长是第一个字符串
    let res = strs[0]

    for(let i = 1; i < strs.length; i++) {
        // 每次更新最长前缀： （这个前缀只会越来越小或者不变，不会边变长）
        res = getLongestCommonPrefix(res, strs[i])
    }

    return res
};

function getLongestCommonPrefix(str1, str2) {
    let str = ''
    for(let i = 0; i < Math.min(str1.length, str2.length); i++) {
        if(str1[i] === str2[i]) str += str1[i]
        else break
    }

    return str
}
```