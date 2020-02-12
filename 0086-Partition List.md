
### Partition List

Given a linked list and a value *x*, partition it such that all nodes less than *x* come before nodes greater than or equal to *x*.

You should preserve the original relative order of the nodes in each of the two partitions.

__Example:__
```
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5
```

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
    func partition(_ head: ListNode?, _ x: Int) -> ListNode? {
        let beforeHead = ListNode(0), afterHead = ListNode(0)
        var before : ListNode? = beforeHead, after : ListNode? = afterHead
        var head = head
        while let curr = head {
            if curr.val < x {
                before?.next = curr
                before = before?.next
            } else {
                after?.next = curr
                after = after?.next
            }
            head = head?.next
        }
        before?.next = afterHead.next
        after?.next = nil
        return beforeHead.next
    }
}
```