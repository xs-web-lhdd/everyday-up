##### 剑指 Offer 22. 链表倒数第k个节点

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var getKthFromEnd = function(head, k) {
    if(!head) return head
    var fast = head
    while(k) {
        fast = fast.next
        k--
    }

    var cur = head
    while(fast) {
        fast = fast.next
        cur = cur.next
    }

    return cur
};
```

