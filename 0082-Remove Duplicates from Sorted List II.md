
### Remove Duplicates from Sorted List II

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only *distinct* numbers from the original list.

Return the linked list sorted as well.

__Example 1:__
```
Input: 1->2->3->3->4->4->5
Output: 1->2->5
```
__Example 2:__
```
Input: 1->1->1->2->3
Output: 2->3
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
        
        // Create a result node in case the first node needs to be
        // removed due to duplicated values
        let result = ListNode(0)
        result.next = head
        
        // Keep curr as a reference to the node prior to the node
        // we want to check for duplicates
        var curr : ListNode? = result
        while let _ = curr {
            
            // Check if the next node has duplicates:
            // If not: keep the next node as it is a non-duplicate
            // Otherwise: drop the duplicate node & move on to the next node 
            if duplicatesOf(curr?.next) == 0 {
                curr = curr?.next
            } else {
                curr?.next = curr?.next?.next
            }
        }
        return result.next
    }
    
    // Helper method to consolidate duplicated nodes and
    // return the number of duplicates
    func duplicatesOf(_ node: ListNode?) -> Int {
        var node = node, count = 0
        while let curr = node, let next = curr.next, curr.val == next.val {
            count+=1
            curr.next = next.next
        }
        return count
    }
}
```