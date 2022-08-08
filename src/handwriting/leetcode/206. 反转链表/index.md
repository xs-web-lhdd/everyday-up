##### 206. 反转链表

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
var reverseList = function(head) {
    if(!head) return head
    let hair = new ListNode(), pre = hair
    let flag = head, cur = head
    let next = head.next

    while(next) {
        cur.next = pre
        pre = cur
        cur = next
        next = next.next
    }
    cur.next = pre
    flag.next = null
    
    return cur
};
```

