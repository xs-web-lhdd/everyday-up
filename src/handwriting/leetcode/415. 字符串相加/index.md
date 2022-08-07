##### 415. 字符串相加

```js
/**
 * @param {string} num1
 * @param {string} num2
 * @return {string}
 */
var addStrings = function(num1, num2) {
    let i = num1.length - 1
    let j = num2.length - 1

    var flag = 0
    var ans = ''
    while(j >= 0 || i >= 0 || flag) {
        var m = num1[i] ? Number(num1[i]) : 0
        var n = num2[j] ? Number(num2[j]) : 0
        var sum = m + n + flag
        if(sum >= 10) {
            ans = (sum % 10) + ans
            flag = 1
        } else {
            ans = sum + ans
            flag = 0
        }
        i--
        j--
    }

    return ans
};
```

