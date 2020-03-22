
### Reorder List

Given a singly linked list *L: L0→L1→…→Ln-1→Ln*,
reorder it to: *L0→Ln→L1→Ln-1→L2→Ln-2→…*

You may __not__ modify the values in the list's nodes, only nodes itself may be changed.

__Example 1:__
```
Given 1->2->3->4, reorder it to 1->4->2->3.
```
__Example 2:__
```
Given 1->2->3->4->5, reorder it to 1->5->2->4->3.
```

### Solution
__O(n) Time, O(1) Space - New List:__
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
    func reorderList(_ head: ListNode?) {
        
        let mid = findMid(head), result = ListNode(0)
        var l1 = head, l2 = reverse(mid?.next), curr : ListNode? = result
        mid?.next = nil
        
        while let node1 = l1, let node2 = l2 {
            let next1 = node1.next, next2 = node2.next
            curr?.next = node1
            curr = curr?.next
            curr?.next = node2
            curr = curr?.next
            l1 = next1
            l2 = next2
        }
        curr?.next = l1 ?? l2
        
        var head = head
        head = result.next
    }
    
    func findMid(_ head: ListNode?) -> ListNode? {
        var fast = head, slow = head
        while let _ = fast?.next {
            fast = fast?.next?.next
            slow = slow?.next
        }
        return slow
    }
    
    func reverse(_ head: ListNode?) -> ListNode? {
        guard let node = head, let next = node.next else { return head }
        let reversed = reverse(next)
        next.next = node
        node.next = nil
        return reversed
    }
}
```
__O(n) Time, O(1) Space - Same List:__
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
    func reorderList(_ head: ListNode?) {
        
        let mid = findMid(head), result = ListNode(0)
        var l1 = head, l2 = reverse(mid?.next), curr : ListNode? = result
        mid?.next = nil

        while let node1 = l1, let node2 = l2 {
            let next1 = node1.next, next2 = node2.next
            node1.next = node2
            node2.next = next1
            l1 = next1
            l2 = next2
        }
        if let node2 = l2 {
            l1?.next = node2
        }
    }
    
    func findMid(_ head: ListNode?) -> ListNode? {
        var fast = head, slow = head
        while let _ = fast?.next {
            fast = fast?.next?.next
            slow = slow?.next
        }
        return slow
    }
    
    func reverse(_ head: ListNode?) -> ListNode? {
        guard let node = head, let next = node.next else { return head }
        let reversed = reverse(next)
        next.next = node
        node.next = nil
        return reversed
    }
}
```