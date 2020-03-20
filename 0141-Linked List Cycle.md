
### Linked List Cycle

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed)</br> 
in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

__Example 1:__
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```
__Example 2:__
```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```
__Example 3:__
```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

__Follow up:__
Can you solve it using O(1) (i.e. constant) memory?

### Solution
__O(n) Time, O(n) Space - HashSet:__
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

extension ListNode : Hashable {
    public static func == (lhs: ListNode, rhs: ListNode) -> Bool {
        return lhs === rhs
    }
    public func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }
}

class Solution {
    func hasCycle(_ head: ListNode?) -> Bool {
        var curr = head, seen : Set<ListNode> = []
        while let node = curr {
            if seen.contains(node) {
                return true
            }
            seen.insert(node)
            curr = node.next
        }
        return false
    }
}
```
__O(n) Time, O(1) Space - Floyd's Tortoise and Hare:__
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
    func hasCycle(_ head: ListNode?) -> Bool {
        var slow = head, fast = head
        while let node = fast, let next = node.next {
            slow = slow?.next
            fast = fast?.next?.next
            if slow === fast {
                return true
            }
        }
        return false
    }
}
```