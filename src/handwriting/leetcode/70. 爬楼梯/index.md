##### 70. 爬楼梯

```js
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function(n) {
    if(n < 0) return 0
    if(n === 1) return 1

    var n1 = 1
    var n2 = 1
    var res = 0
    for(var i = 2; i <= n; i++) {
        res = n1 + n2
        n2 = n1
        n1 = res
    }

    return res
};
```

