
### Palindrome Linked List

Given the `head` of a singly linked list, return `true` if it is a palindrome or `false` otherwise.

__Example 1:__

![question_234-0.jpg](../images/question_234-0.jpg)
```
Input: head = [1,2,2,1]
Output: true
```
__Example 2:__

![question_234-1.jpg](../images/question_234-1.jpg)
```
Input: head = [1,2]
Output: false
```

__Constraints:__
* The number of nodes in the list is in the range `[1, pow(10, 5)]`.
* `0 <= Node.val <= 9`

__Follow up:__ 
* Could you do it in `O(n)` time and `O(1)` space?

### Solution
__O(head) Time, O(head) Space - Array Storage:__
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
    func isPalindrome(_ head: ListNode?) -> Bool {
        var values: [Int] = []
        var head: ListNode? = head
        while let node = head {
            values.append(node.val)
            head = node.next
        }

        var start: Int = 0
        var end: Int = values.count - 1
        while start < end {
            if values[start] != values[end] {
                return false
            } else {
                start += 1
                end -= 1
            }
        }
        return true
    }
}
```
__O(head) Time, O(1) Space - Reverse List Mid-Way:__
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
    func isPalindrome(_ head: ListNode?) -> Bool {
        var head: ListNode? = head
        var reversed: ListNode? = reverse(findMid(head))
        while reversed != nil {
            if head?.val != reversed?.val {
                return false
            } else {
                head = head?.next
                reversed = reversed?.next
            }
        }
        return true
    }

    func findMid(_ node: ListNode?) -> ListNode? {
        var slow: ListNode? = node
        var fast: ListNode? = node
        while fast != nil {
            slow = slow?.next
            fast = fast?.next?.next
        }
        return slow
    }

    func reverse(_ node: ListNode?) -> ListNode? {
        guard let node, let next = node.next else { return node }
        let reversed: ListNode? = reverse(next)
        next.next = node
        node.next = nil
        return reversed
    }
}
```
