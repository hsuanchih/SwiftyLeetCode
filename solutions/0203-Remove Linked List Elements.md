
### Remove Linked List Elements

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return the new head.

__Example 1:__
![question_203.jpg](../images/question_203.jpg)
```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```
__Example 2:__
```
Input: head = [], val = 1
Output: []
```
__Example 3:__
```
Input: head = [7,7,7,7], val = 7
Output: []
```

__Constraints:__
* The number of nodes in the list is in the range `[0, pow(10, 4)]`.
* `1 <= Node.val <= 50`
* `0 <= val <= 50`

### Solution
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
    func removeElements(_ head: ListNode?, _ val: Int) -> ListNode? {
        let dummyHead: ListNode = ListNode(0)
        dummyHead.next = head
        var curr: ListNode? = dummyHead
        while let next = curr?.next {
            if next.val == val {
                curr?.next = next.next
            } else {
                curr = next
            }
        }
        return dummyHead.next
    }
}
```