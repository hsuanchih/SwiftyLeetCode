
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
        var list1: ListNode? = list1
        var list2: ListNode? = list2
        let dummyHead: ListNode = ListNode(0)
        var curr: ListNode? = dummyHead
        while let l1 = list1, let l2 = list2 {
            if l1.val < l2.val {
                curr?.next = l1
                list1 = list1?.next
            } else {
                curr?.next = l2
                list2 = list2?.next
            }
            curr = curr?.next
        }
        if let l1 = list1 {
            curr?.next = l1
        } else if let l2 = list2 {
            curr?.next = l2
        }
        return dummyHead.next
    }
}
```
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
        while p1 != nil || p2 != nil {
            switch (p1, p2) {
            case (.some(let node1), .some(let node2)):
                if node1.val < node2.val {
                    curr?.next = node1
                    p1 = node1.next
                } else {
                    curr?.next = node2
                    p2 = node2.next
                }
                curr = curr?.next
            case (.some(let node1), .none):
                curr?.next = node1
                return dummyHead.next
            case (.none, .some(let node2)):
                curr?.next = node2
                p2 = node2.next
                return dummyHead.next
            case (.none, .none):
                fatalError()
            }
        }
        return dummyHead.next
    }
}
```
