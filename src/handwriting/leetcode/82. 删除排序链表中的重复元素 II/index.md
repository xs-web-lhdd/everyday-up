##### 82. 删除排序链表中的重复元素 II:
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
var deleteDuplicates = function (head) {
  if(!head) return head
  let hair = new ListNode(-1, head)
  let pre = hair,
    cur = head
  while (cur && cur.next) {
    if (cur.val !== cur.next.val) {
      pre = pre.next
      cur = cur.next
    } else {
      while (cur && cur.next && (cur.val === cur.next.val)) {
        cur = cur.next
      }
      pre.next = cur.next
      cur = cur.next
    }
  }


  return hair.next
};
```