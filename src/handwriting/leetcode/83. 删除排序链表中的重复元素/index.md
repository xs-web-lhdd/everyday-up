##### 83. 删除排序链表中的重复元素:
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var deleteDuplicates = function(head) {
  // 判断是否存在
  if (!head) return head
  // new 一个虚拟头节点
  var hair = new ListNode(-1, head),
    pre = hair,
    cur = head
  while (cur && cur.next) {
    // 如果cur不等于cur的下一位，将cur和pre均往后移动一位
    if (cur.val !== cur.next.val) {
      pre = pre.next
      cur = cur.next
    } else {
      // 如果cur等于cur的下一位，则只移动cur，直到cur的下一位和cur不相等为止
      while (cur && cur.next && (cur.val === cur.next.val)) {
        cur = cur.next
      }
      // 然后把pre的下一位指向cur的下一位，并把cur往后移一位
      pre.next = cur
    }
  }

  return hair.next
};
```