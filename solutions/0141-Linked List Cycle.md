
### Linked List Cycle

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's next pointer is connected to. Note that `pos` is not passed as a parameter.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

__Example 1:__
![example1](images/question_141-0.png)
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```
__Example 2:__
![example2](images/question_141-1.png)
```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```
__Example 3:__
![example3](images/question_141-2.png)
```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

__Constraints:__
* The number of the nodes in the list is in the range `[0, pow(10, 4)]`.
* `-pow(10, 5) <= Node.val <= pow(10, 5)`
* `pos` is `-1` or a valid index in the linked-list.
 
__Follow up:__
* Can you solve it using `O(1)` (i.e. constant) memory?

### Solution
__O(pow(head, 2)) Time, O(head) Space - Array:__
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
        var visited: [ListNode] = []
        var head: ListNode? = head
        while let node = head {
            if visited.contains(where: { $0 === node }) {
                return true
            } else {
                visited.append(node)
                head = node.next
            }
        }
        return false
    }
}
```
__O(head) Time, O(head) Space - HashSet:__
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
extension ListNode: Hashable {
    public func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }

    public static func == (lhs: ListNode, rhs: ListNode) -> Bool {
        lhs === rhs
    }
}

class Solution {
    func hasCycle(_ head: ListNode?) -> Bool {
        var visited: Set<ListNode> = []
        var head: ListNode? = head
        while let node = head {
            if visited.contains(node) {
                return true
            } else {
                visited.insert(node)
                head = node.next
            }
        }
        return false
    }
}
```
__O(head) Time, O(1) Space - Floyd's Tortoise and Hare:__
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
        var slow: ListNode? = head
        var fast: ListNode? = head?.next
        while let node = fast {
            if slow === fast {
                return true
            } else {
                slow = slow?.next
                fast = node.next?.next
            }
        }
        return false
    }
}
```
