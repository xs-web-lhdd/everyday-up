##### 104. 二叉树的最大深度:
DFS:
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
var maxDepth = function(root) {
    if(!root) return 0
    return dfs(root, 0)
};

function dfs(root, height) {
    if(!root) return height

    return Math.max(dfs(root.left, height+1), dfs(root.right, height+1))
}
```

BFS:
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
var maxDepth = function(root) {
    if(!root) return 0
    let queue = [root]
    let count = 0
    while(queue.length) {
        let length = queue.length
        count++
        for(let i = 0; i < length; i++) {
            const node = queue.shift()
            node.left && queue.push(node.left)
            node.right && queue.push(node.right)
        }
    }

    return count
}
```