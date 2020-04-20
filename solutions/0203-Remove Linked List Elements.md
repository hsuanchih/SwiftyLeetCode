
### Remove Linked List Elements

Remove all elements from a linked list of integers that have value __val__.

__Example:__
```
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5
```

### Solution
```Swift
class Solution {
    func removeElements(_ head: ListNode?, _ val: Int) -> ListNode? {
        let result = ListNode(0)
        result.next = head
        var curr : ListNode? = result
        while let node = curr?.next {
            if node.val == val {
                curr?.next = curr?.next?.next
            } else {
                curr = curr?.next
            }
        }
        return result.next
    }
}
```