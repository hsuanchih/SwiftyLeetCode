
### Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list *k* at a time and return its modified list.

*k* is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of *k* then left-out nodes in the end should remain as it is.

__Example:__
Given this linked list: `1->2->3->4->5`
For k = 2, you should return: `2->1->4->3->5`
For k = 3, you should return: `3->2->1->4->5`

__Note:__
* Only constant extra memory is allowed.
* You may not alter the values in the list's nodes, only nodes itself may be changed.

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
    func reverseKGroup(_ head: ListNode?, _ k: Int) -> ListNode? {
        guard var head = head else { return nil }
        guard var node = node(number: k, from: head) else { return head }
        let result = reverseKGroup(node.next, k)
        node.next = nil
        let reversed = reverse(head)
        head.next = result
        return reversed
    }
    
    func node(number k: Int, from node: ListNode) -> ListNode? {
        var curr : ListNode? = node, count = 1
        while let _ = curr, count < k {
            curr = curr?.next
            count+=1
        }
        return curr
    }
    
    func reverse(_ node: ListNode?) -> ListNode? {
        var prev : ListNode?, node = node
        while let curr = node {
            var next = curr.next
            node?.next = prev
            prev = node
            node = next
        }
        return prev
    }
}
```