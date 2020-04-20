
### Insertion Sort List

Sort a linked list using insertion sort.

![example](images/question_147.gif)

A graphical example of insertion sort. The partial sorted list (black) initially contains only the first element in the list.
With each iteration one element (red) is removed from the input data and inserted in-place into the sorted list

__Algorithm of Insertion Sort:__
1. Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
3. It repeats until no input elements remain.

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
__O(n^2) - Node Swap:__
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
    func insertionSortList(_ head: ListNode?) -> ListNode? {
        let result = ListNode(0)
        var curr = head
        while let node = curr {
            
            // Keep a reference to current node's next
            let next = node.next
            
            // Insert current node into the right place in result
            var iter : ListNode? = result
            // Iterate through result to find the first node greater than
            // the current node
            while let next = iter?.next, next.val <= node.val {
                iter = next
            }
            let temp = iter?.next
            iter?.next = node
            node.next = temp
            
            // Assign current node's next as the current node
            curr = next
        }
        return result.next
    }
}
```
