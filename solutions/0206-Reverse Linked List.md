
### Reverse Linked List

Given the `head` of a singly linked list, reverse the list, and return the reversed list.

__Example 1:__
![question_206-0.jpg](../images/question_206-0.jpg)
```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```
__Example 2:__
![question_206-1.jpg](../images/question_206-1.jpg)
```
Input: head = [1,2]
Output: [2,1]
```
__Example 3:__
```
Input: head = []
Output: []
```

__Constraints:__
* The number of nodes in the list is the range `[0, 5000]`.
* `-5000 <= Node.val <= 5000`

__Follow up:__
A linked list can be reversed either iteratively or recursively. Could you implement both?

### Solution
__Iterative:__
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
    func reverseList(_ head: ListNode?) -> ListNode? {
        var prev: ListNode? = nil
        var curr: ListNode? = head

        while let node = curr {
            let next: ListNode? = node.next
            node.next = prev
            prev = curr
            curr = next
        }
        return prev
    }
}
```
__Recursive:__
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
    func reverseList(_ head: ListNode?) -> ListNode? {
        guard let head else { return nil }
        if let next = head.next {
            let reversed = reverseList(next)
            next.next = head
            head.next = nil
            return reversed
        } else {
            return head
        }
    }
}
```