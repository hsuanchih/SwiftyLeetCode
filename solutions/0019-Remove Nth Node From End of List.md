
### Remove Nth Node From End of List

Given a linked list, remove the n-th node from the end of list and return its head.

__Example:__
```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```
__Note:__
Given n will always be valid.

__Follow up:__
Could you do this in one pass?

### Solution
__O(n):__
```Swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.next = nil
 *     }
 * }
 */
class Solution {
    func removeNthFromEnd(_ head: ListNode?, _ n: Int) -> ListNode? {
        let result = ListNode(0)
        result.next = head
        var fast : ListNode? = result
        for _ in 0...n {
            fast = fast?.next
        }
        var slow : ListNode? = result
        while let _ = fast {
            slow = slow?.next
            fast = fast?.next
        }
        slow?.next = slow?.next?.next
        return result.next
    }
}
```
