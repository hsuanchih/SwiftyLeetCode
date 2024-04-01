
### Merge Two Sorted Lists

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

__Example 1:__
![question_21.jpg](../images/question_21.jpg)
```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```
__Example 2:__
```
Input: list1 = [], list2 = []
Output: []
```
__Example 3:__
```
Input: list1 = [], list2 = [0]
Output: [0]
```

__Constraints:__
* The number of nodes in both lists is in the range `[0, 50]`.
* `-100 <= Node.val <= 100`
* Both `list1` and `list2` are sorted in __non-decreasing__ order.

### Solution
__O(list1 + list2) Time:__
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
    func mergeTwoLists(_ list1: ListNode?, _ list2: ListNode?) -> ListNode? {
        let dummyHead: ListNode = ListNode(0)
        var curr: ListNode? = dummyHead
        var p1: ListNode? = list1
        var p2: ListNode? = list2
        while let node1 = p1, let node2 = p2 {
            if node1.val < node2.val {
                curr?.next = node1
                p1 = node1.next
            } else {
                curr?.next = node2
                p2 = node2.next
            }
            curr = curr?.next
        }
        if let node1 = p1 {
            curr?.next = node1
        } else {
            curr?.next = p2
        }
        return dummyHead.next
    }
}
```
__O(l1+l2):__
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
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        let result: ListNode? = .init(0)
        var curr = result, l1 = l1, l2 = l2
        while l1 != nil || l2 != nil {
            if (l1?.val ?? Int.max) < (l2?.val ?? Int.max) {
                curr?.next = l1
                l1 = l1?.next
            } else {
                curr?.next = l2
                l2 = l2?.next
            }
            curr = curr?.next
        }
        return result?.next
    }
}
```