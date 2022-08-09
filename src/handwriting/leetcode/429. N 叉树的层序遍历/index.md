##### 429. N 叉树的层序遍历
```js
/**
 * // Definition for a Node.
 * function Node(val,children) {
 *    this.val = val;
 *    this.children = children;
 * };
 */

/**
 * @param {Node|null} root
 * @return {number[][]}
 */
var levelOrder = function(root) {
    if(!root) return []
    var res = []
    var queue = [root]
    while(queue.length) {
        var arr = []
        var length = queue.length
        for(var i = 0; i < length; i++) {
            var node = queue.shift()
            arr.push(node.val)
            if(node.children.length) {
                for(var j = 0; j < node.children.length; j++) {
                    if(node.children[j]) queue.push(node.children[j])
                }
            }
        }
        res.push(arr)
    }

    return res
};
```