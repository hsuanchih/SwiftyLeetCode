
### Sort List

Sort a linked list in O(*n* log *n*) time using constant space complexity.

__Example 1:__
```
Input: 4->2->1->3
Output: 1->2->3->4
```
__Example 2:__
```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
```

### Solution
__O(n*log(n)) Merge Sort:__
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
    func sortList(_ head: ListNode?) -> ListNode? {

        // If we've sub-divided the linked list to a single node,
        // the list is by nature sorted, return the node
        if head?.next == nil { return head }

        // Find the middle ListNode & divide the linked list into left sub-list & right sub-list
        let mid = findMid(head)
        let left = head, right = mid?.next
        mid?.next = nil

        // Recursively sort the left & right sub-lists, and return the merged sorted lists
        return merge(sortList(left), sortList(right))
    }
    
    // Helper method to combine 2 sorted linked lists
    func merge(_ left: ListNode?, _ right: ListNode?) -> ListNode? {
        let result = ListNode(0)
        var left = left, right = right, curr : ListNode? = result
        while let lVal = left?.val, let rVal = right?.val {
            if lVal < rVal {
                curr?.next = left
                left = left?.next
            } else {
                curr?.next = right
                right = right?.next
            }
            curr = curr?.next
        }

        // Append the leftover list from either left or right to the end of the result
        switch (left, right) {
            case (.some(_), _):
            curr?.next = left
            case (_, .some(_)):
            curr?.next = right
            default:
            break
        }
        return result.next
    }
    
    // Helper method to find the middle ListNode given a linked list
    func findMid(_ head: ListNode?) -> ListNode? {
        var slow = head, fast = head
        while let nextFast = fast?.next?.next {
            fast = nextFast
            slow = slow?.next
        }
        return slow
    }
}
```