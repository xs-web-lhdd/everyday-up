##### 139. 单词拆分:
DP: B 站视频
```js
/**
 * @param {string} s
 * @param {string[]} wordDict
 * @return {boolean}
 */
var wordBreak = function(s, wordDict) {
    let dp = new Array(s.length + 1).fill(false)
    s = ' ' + s
    dp[0] = true
    for(let i = 1; i < s.length; i++) {
        for(let j = 0; j <= i; j++) {
            if(dp[j] && wordDict.includes(s.slice(j + 1, i + 1))) {
                dp[i] = true
                break
            }
        }
    }

    return dp[dp.length - 1]
};
```