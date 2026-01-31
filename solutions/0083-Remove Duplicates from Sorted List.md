
### Remove Duplicates from Sorted List

Given the `head` of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list __sorted__ as well.

__Example 1:__
![question_83-0.jpg](../images/question_83-0.jpg)
```
Input: head = [1,1,2]
Output: [1,2]
```
__Example 2:__
![question_83-1.jpg](../images/question_83-1.jpg)
```
Input: head = [1,1,2,3,3]
Output: [1,2,3]
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
    func deleteDuplicates(_ head: ListNode?) -> ListNode? {
        var curr = head
        while let node = curr, let next = node.next {
            if node.val == next.val {
                curr?.next = next.next
            } else {
                curr = curr?.next
            }
        }
        return head
    }
}
```