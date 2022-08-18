##### 113. 路径总和 II：
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
 * @param {number} targetSum
 * @return {number[][]}
 */
var pathSum = function(root, targetSum) {
    if(!root) return []
    let res = []
    dfs(root, targetSum, res, [])

    return res
};

function dfs(root, target, res, arr) {
    let sliceArr = arr.slice()
    // 叶子节点并且满足相加为 targetSum
    if(root.val === target && !root.left && !root.right) {
        sliceArr.push(target)
        res.push(sliceArr)
        return
    }

    if(root.left) {
        let slice = arr.slice()
        slice.push(root.val)
        dfs(root.left, target - root.val, res, slice)
    }
    if(root.right) {
        let slice = arr.slice()
        slice.push(root.val)
        dfs(root.right, target - root.val, res, slice)
    }
}
```