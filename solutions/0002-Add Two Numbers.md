
### Add Two Numbers

You are given two __non-empty__ linked lists representing two non-negative integers.</br> 
The digits are stored in __reverse order__ and each of their nodes contain a single digit.</br> 
Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

__Example:__
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```
### Solution
__O(l1+l2) Time, O(l1+l2) Space:__
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
        let result = ListNode(0)
        
        var carry: Int = 0, 
        l1: ListNode? = l1, 
        l2: ListNode? = l2, 
        curr: ListNode? = result
        
        while l1 != nil || l2 != nil {
        
            // Compute sum & carry
            let sum = (l1?.val ?? 0) + (l2?.val ?? 0) + carry
            carry = sum / 10
            curr?.next = ListNode(sum % 10)

            // Advance l1, l2 & curr
            l1 = l1?.next
            l2 = l2?.next
            curr = curr?.next
        }
        
        // Process the final carry
        if carry != 0 {
            curr?.next = ListNode(carry)
        }
        
        return result.next
    }
}
```
