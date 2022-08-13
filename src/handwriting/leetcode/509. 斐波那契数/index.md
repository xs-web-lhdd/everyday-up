##### 509. 斐波那契数:
```js
/**
 * @param {number} n
 * @return {number}
 */
var fib = function(n) {
    if(n == 0 || n == 1) return n
    let p1 = 0
    let p2 = 1
    let res
    for(let i = 2; i <=n; i++) {
        res = p1 + p2
        p1 = p2
        p2 = res
    }

    return res
};
```