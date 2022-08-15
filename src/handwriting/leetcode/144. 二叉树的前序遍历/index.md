##### 144. 二叉树的前序遍历
栈版本：
```js
var preorderTraversal = function(root) {
    let res = [];
    if(!root){
        return res;
    }
    let stack = [root];
    let cur;
    while(stack.length){
        cur = stack.pop();
        res.push(cur.val); // 后续遍历这里是 unshift
        cur.right && stack.push(cur.right); // 后序遍历 14 15 互换位置
        cur.left && stack.push(cur.left);
    }
    return res;
};
```