##### 20. 有效的括号:
```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    var stack = []
    for(var item of s) {
        if(item === '(' || item === '{' || item === '[') stack.push(item)
        else {
            if(item === ')' && stack[stack.length - 1] === '(') stack.pop()
            else if(item === ']' && stack[stack.length - 1] === '[') stack.pop()
            else if(item === '}' && stack[stack.length - 1] === '{') stack.pop()
            else stack.push(item)
        }
    }

    return stack.length == 0
};
```