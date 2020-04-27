
### Linked List Random Node

Given a singly linked list, return a random node's value from the linked list.</br> 
Each node must have the __same probability__ of being chosen.

__Follow up:__
What if the linked list is extremely large and its length is unknown to you?</br> 
Could you solve this efficiently without using extra space?

__Example:__
```
// Init a singly linked list [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
solution.getRandom();
```

### Solution
__O(n) Time Access, O(n) Space - Array:__
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
    
    private var values : [Int] = []

    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    init(_ head: ListNode?) {
        var curr = head
        while let node = curr {
            values.append(node.val)
            curr = node.next
        }
    }
    
    /** Returns a random node's value. */
    func getRandom() -> Int {
        return values[Int.random(in: 0..<values.count)]
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * let obj = Solution(head)
 * let ret_1: Int = obj.getRandom()
 */
```
__O(n) Time Access, O(1) Space - Reservoir Sampling:__
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
    
    private let head : ListNode?

    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    init(_ head: ListNode?) {
        self.head = head
    }
    
    /** Returns a random node's value. */
    func getRandom() -> Int {
        var curr = head, i = 0, result = curr?.val
        while let node = curr {
            if Int.random(in: 0..<i+1) == i {
                result = node.val
            }
            i+=1
            curr = node.next
        }
        return result ?? -1
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * let obj = Solution(head)
 * let ret_1: Int = obj.getRandom()
 */
```