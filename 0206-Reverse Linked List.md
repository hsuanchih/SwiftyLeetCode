
### Reverse Linked List

Reverse a singly linked list.

__Example:__
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

__Follow up:__
A linked list can be reversed either iteratively or recursively. Could you implement both?

### Solution
__Iterative:__
```Swift
class Solution {
    func reverseList(_ head: ListNode?) -> ListNode? {
        var prev : ListNode?, curr = node
        while let node = curr {
            let next = node.next
            node.next = prev
            prev = node
            curr = next
        }
        return prev
    }
}
```
__Recursive:__
```Swift
class Solution {
    func reverseList(_ head: ListNode?) -> ListNode? {
        guard let curr = node, let next = curr.next else { return node }
        let reversed = recursive(next)
        next.next = curr
        curr.next = nil
        return reversed
    }
}
```