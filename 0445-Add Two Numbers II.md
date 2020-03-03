
### Add Two Numbers II

You are given two __non-empty__ linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

__Follow up:__
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

__Example:__
```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

### Solution
__O(l1+l2) Time, O(max(l1,l2)) Space:__
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
    func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        var l1 = reverse(l1), l2 = reverse(l2), result = ListNode(0)
        var carry = 0, curr : ListNode? = result
        while l1 != nil || l2 != nil {
            let sum = (l1?.val ?? 0) + (l2?.val ?? 0) + carry
            curr?.next = ListNode(sum%10)
            carry = sum/10
            l1 = l1?.next
            l2 = l2?.next
            curr = curr?.next
        }
        if carry > 0 {
            curr?.next = ListNode(carry)
        }
        return reverse(result.next)
    }
    
    func reverse(_ node: ListNode?) -> ListNode? {
        guard let curr = node, let next = curr.next else { return node }
        let reversed = reverse(curr.next)
        curr.next?.next = curr
        curr.next = nil
        return reversed
    }
}
```