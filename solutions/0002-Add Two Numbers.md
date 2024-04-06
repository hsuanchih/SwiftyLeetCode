
### Add Two Numbers

You are given two __non-empty__ linked lists representing two non-negative integers.</br> 
The digits are stored in __reverse order__ and each of their nodes contain a single digit.</br> 
Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number `0` itself.

__Example 1:__

![question_2.jpg](../images/question_2.jpg)
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```
__Example 2:__
```
Input: l1 = [0], l2 = [0]
Output: [0]
```
__Example 3:__
```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```
 
__Constraints:__
* The number of nodes in each linked list is in the range `[1, 100]`.
* `0 <= Node.val <= 9`

### Solution
__O(l1 + l2) Time, O(l1 + l2) Space:__
```Swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init() { self.val = 0; self.next = nil; }
 *     public init(_ val: Int) { self.val = val; self.next = nil; }
 *     public init(_ val: Int, _ next: ListNode?) { self.val = val; self.next = next; }
 * }
 */
class Solution {
    func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        var carry: Int = 0
        var dummyHead: ListNode = ListNode(0)
        var curr: ListNode? = dummyHead
        var l1: ListNode? = l1
        var l2: ListNode? = l2
        while l1 != nil || l2 != nil || carry != 0 {
            let val: Int = (l1?.val ?? 0) + (l2?.val ?? 0) + carry
            carry = val / 10
            curr?.next = ListNode(val % 10)
            curr = curr?.next
            l1 = l1?.next
            l2 = l2?.next
        }
        return dummyHead.next
    }
}
```
