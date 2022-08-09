##### 230. 二叉搜索树中第K小的元素
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
 * @param {number} k
 * @return {number}
 */
var kthSmallest = function (root, k) {
  var arr = []
  preOrder(root, arr)


  return arr[k - 1]
};

function preOrder(root, arr) {
  if (!root) return
  preOrder(root.left, arr)
  arr.push(root.val)
  preOrder(root.right, arr)
}
```