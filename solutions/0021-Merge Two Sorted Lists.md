
### Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

__Example:__
```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

### Solution
__O(l1+l2):__
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
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        let result = ListNode(0)
        var iter : ListNode? = result, l1 = l1, l2 = l2
        while let l = l1, let r = l2 {
            if l.val < r.val {
                iter?.next = ListNode(l.val)
                l1 = l1?.next
            } else {
                iter?.next = ListNode(r.val)
                l2 = l2?.next
            }
            iter = iter?.next
        }
        if let l1 = l1 {
            iter?.next = l1
        }
        if let l2 = l2 {
            iter?.next = l2
        }
        return result.next
    }
}
```
__O(l1+l2):__
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
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        let result: ListNode? = .init(0)
        var curr = result, l1 = l1, l2 = l2
        while l1 != nil || l2 != nil {
            if (l1?.val ?? Int.max) < (l2?.val ?? Int.max) {
                curr?.next = l1
                l1 = l1?.next
            } else {
                curr?.next = l2
                l2 = l2?.next
            }
            curr = curr?.next
        }
        return result?.next
    }
}
```