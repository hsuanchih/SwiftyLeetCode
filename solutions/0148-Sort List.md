
### Sort List

Given the `head` of a linked list, return the list after sorting it in __ascending order__.

__Example 1:__

![question_148-0.jpg](../images/question_148-0.jpg)
```
Input: head = [4,2,1,3]
Output: [1,2,3,4]
```
__Example 2:__

![question_148-1.jpg](../images/question_148-1.jpg)
```
Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
```
__Example 3:__
```
Input: head = []
Output: []
```

__Constraints:__
* The number of nodes in the list is in the range `[0, 5 * pow(10, 4)]`.
* `-pow(10, 5) <= Node.val <= pow(10, 5)`
 

__Follow up:__ 
* Can you sort the linked list in `O(n logn)` time and `O(1)` memory (i.e. constant space)?

### Solution
__O(n * log\[2\](n)) Merge Sort:__
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
    func sortList(_ head: ListNode?) -> ListNode? {
        guard let head else { return nil }
        if head.next == nil {
            // If we've sub-divided the linked list to a single node,
            // the list is by nature sorted, return the node
            return head
        } else {
            // Find the middle ListNode & divide the linked list into left sub-list & right sub-list
            let mid: ListNode? = findMid(head)
            let next: ListNode? = mid?.next
            mid?.next = nil

            // Recursively sort the left & right sub-lists, and return the merged sorted lists
            return merge(sortList(head), sortList(next))
        }
    }
    
    // Helper method to combine 2 sorted linked lists
    func merge(_ left: ListNode?, _ right: ListNode?) -> ListNode? {
        let dummyHead: ListNode = ListNode(0)
        var curr: ListNode? = dummyHead
        var left: ListNode? = left
        var right: ListNode? = right
        while let leftNode = left, let rightNode = right {
            if leftNode.val < rightNode.val {
                curr?.next = leftNode
                left = left?.next
            } else {
                curr?.next = rightNode
                right = right?.next
            }
            curr = curr?.next
        }

        // Append the leftover list from either left or right to the end of the result
        if let left {
            curr?.next = left
        } else if let right {
            curr?.next = right
        }
        return dummyHead.next
    }
    
    // Helper method to find the middle ListNode given a linked list
    func findMid(_ node: ListNode) -> ListNode? {
        var fast: ListNode? = node.next?.next
        var slow: ListNode? = node
        while let node = fast {
            fast = fast?.next?.next
            slow = slow?.next
        }
        return slow
    }
}
```