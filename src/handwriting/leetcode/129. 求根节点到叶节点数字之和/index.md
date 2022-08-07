##### 129. 求根节点到叶节点数字之和

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var sumNumbers = function(root) {
    var strArr = []
    dfs(root, strArr, '')

    console.log(strArr)
    var ans = 0
    strArr.forEach(item => {
        ans += Number(item)
    })

    return ans
};

function dfs(root, strArr, str) {
    if(!root.left && !root.right) {
        strArr.push(str + root.val)
        return
    }
    if(root.left) {
        dfs(root.left, strArr, str + root.val)
    }
    if(root.right) {
        dfs(root.right, strArr, str + root.val)
    }

}
```

