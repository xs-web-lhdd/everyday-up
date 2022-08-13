##### 21. 合并两个有序链表:
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} list1
 * @param {ListNode} list2
 * @return {ListNode}
 */
var mergeTwoLists = function(list1, list2) {
    var hair = new ListNode(-1), p = hair
    var p1 = list1, p2 = list2

    while(p1 && p2) {
        if(p1.val >= p2.val) {
            p.next = p2
            p2 = p2.next
        } else {
            p.next = p1
            p1 = p1.next
        }
        p = p.next
    }
    if(p1) p.next = p1
    if(p2) p.next = p2
    return hair.next
};
```