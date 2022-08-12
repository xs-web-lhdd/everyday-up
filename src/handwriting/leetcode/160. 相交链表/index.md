##### 160. 相交链表:
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function(headA, headB) {
    if(!headA || !headB) return null
    let p = headA, q = headB
    while(p !== q) {
      // 巧妙之处，p === null，这样当两个链表没有相交节点时 null 就是相交节点，并且如果有相交节点，那么相交节点会在 null 之前相遇
       p = p === null ? headB : p.next
       q = q === null ? headA : q.next
    }

    return q
};
```