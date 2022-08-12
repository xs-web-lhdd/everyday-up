##### 343. 整数拆分:
###### dp: 这个题解不错: https://leetcode.cn/problems/integer-break/solution/343-zheng-shu-chai-fen-by-peterz-li-fh9h/
```js
/**
 * @param {number} n
 * @return {number}
 */
var integerBreak = function(n) {
    let dp = new Array(n+1).fill(0)
    dp[2] = 1

    for(let i = 3; i <= n; i++) {
        for(let j = 1; j <= Math.floor(i / 2); j++) {
            dp[i] = Math.max(dp[i], j * (i - j), j * dp[i - j], (i - j) * dp[j])
        }
    }

    return dp[n]
};
```