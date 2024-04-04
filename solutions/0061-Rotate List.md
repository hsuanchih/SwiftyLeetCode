
### Rotate List

Given the `head` of a linked list, rotate the list to the right by `k` places.

__Example 1:__

![question_61-0.jpg](../images/question_61-0.jpg)
```
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
```
__Example 2:__

![question_61-1.jpg](../images/question_61-1.jpg)
```
Input: head = [0,1,2], k = 4
Output: [2,0,1]
```

__Constraints:__
* The number of nodes in the list is in the range `[0, 500]`.
* `-100 <= Node.val <= 100`
* `0 <= k <= 2 * pow(10, 9)`

### Solution
__O(head) Time, O(head) Space:__
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
    func rotateRight(_ head: ListNode?, _ k: Int) -> ListNode? {
        guard head != nil else { return head }
        var nodes: [ListNode] = []
        var curr: ListNode? = head
        while let node = curr {
            nodes.append(node)
            curr = node.next
        }

        let rotation: Int = k % nodes.count 
        if rotation == 0 {
            return head
        } else {
            nodes.last?.next = nodes.first
            nodes[nodes.count - 1 - rotation].next = nil
        }
        return nodes[nodes.count - rotation]
    }
}
```
__O(2 * head) Time, O(1) Space:__
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
    func rotateRight(_ head: ListNode?, _ k: Int) -> ListNode? {
        guard head != nil || k != 0 else { return head }
        var listSize: Int = 1
        var tail: ListNode? = head
        while let next = tail?.next {
            listSize += 1
            tail = next
        }

        let shift: Int = k % listSize
        if shift == 0 {
            return head
        } else {
            var curr: ListNode? = head
            for _ in 1 ..< listSize - shift {
                curr = curr?.next
            }
            let newHead: ListNode? = curr?.next
            curr?.next = nil
            tail?.next = head
            return newHead
        }
    }
}
```