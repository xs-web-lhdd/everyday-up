##### 141. 环形链表
- 哈希表解法：
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
 * @return {boolean}
 */
var hasCycle = function(head) {
  if (!head) return false
  let slow = head
  let map = new Map()
  while (slow) {
    if (map.has(slow)) return true
    map.set(slow, 1)
    slow = slow.next
  }

  return false
};
```

- 快慢指针解法：
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
 * @return {boolean}
 */
 var hasCycle = function(head) {
    if(!head) return false
    let fast = head, slow = head;
    while(fast && fast.next) {
      fast = fast.next.next
      slow = slow.next

      if(fast == slow) {
        return true
      }
    }

    return false
};
```