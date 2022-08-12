##### 94. 二叉树的中序遍历
###### 递归版:
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
 * @return {number[]}
 */
var inorderTraversal = function(root) {
    if(!root) return []
    var arr = []
    dfs(root, arr)

    return arr
};

function dfs(root, arr) {
    if(!root) return
    dfs(root.left, arr)
    arr.push(root.val)
    dfs(root.right, arr)
}
```

###### 栈版: 建议背下来!
```js
var inorderTraversal = function(root) {
    const res = [];
    const stk = [];
    while (root || stk.length) {
        while (root) {
            stk.push(root);
            root = root.left;
        }
        root = stk.pop();
        res.push(root.val);
        root = root.right;
    }
    return res;
};
```