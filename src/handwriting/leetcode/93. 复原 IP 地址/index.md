##### 93. 复原 IP 地址

```js
/**
 * @param {string} s
 * @return {string[]}
 * @solution 官方题解下面的有一篇 js 就很好
 */
 var restoreIpAddresses = function(s) {
    const len = s.length
    if (len < 4 || len > 12) return []
    const result = []

    function dfs(start, p, path) {
        if(p === 4) {
            if(start === len) result.push(path)
            return
        }

        var str = ''
        for(var i = start; i < start + 3; i++) {
            str += s[i]
            if(Number(str) > 255) break
            dfs(i + 1, p + 1, path + str + (p === 3 ? '' : '.'))
            // 如果第一个就是 0，那就没必要继续往后 dfs 了，因为往后续上字符串就是前导 0
            // 还有，这里为啥是 == 而不是 ===，因为字符串和数字
            if(s[start] == 0) break
        }
    }
    dfs(0, 0, '')
    return result
};
```

