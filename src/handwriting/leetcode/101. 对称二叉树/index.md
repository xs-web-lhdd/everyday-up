##### 101. 对称二叉树:
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
 * @return {boolean}
 */
var isSymmetric = function (root) {
  let check = (p, q) => {
    if (p == null || q == null) return p==q;
    // 每个树的右子树都与这棵树的左子树镜像对称
    return p.val == q.val && check(p.left, q.right) && check(p.right, q.left);
  };
  return check(root, root);
};



```