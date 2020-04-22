
### Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head.</br>
You may __not__ modify the values in the list's nodes, only nodes itself may be changed.

__Example:__
```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

### Solution
__Recursive O(n):__
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
    func swapPairs(_ head: ListNode?) -> ListNode? {
        guard let curr = head, let next = curr.next else {
            return head
        }
        let nextPair = swapPairs(next.next)
        curr.next = nextPair
        next.next = curr
        return next
    }
}
```
__Iterative O(n):__
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
    func swapPairs(_ head: ListNode?) -> ListNode? {
        let result = ListNode(0)
        result.next = head
        
        // Use 3 references to traverse the list
        var prev : ListNode? = result,
        curr : ListNode? = head,
        next : ListNode? = curr?.next
        
        while let _ = next {
            curr?.next = next?.next
            next?.next = curr
            prev?.next = next
            prev = curr
            curr = curr?.next
            next = curr?.next
        }
        return result.next
    }
}
```