
### Convert Binary Number in a Linked List to Integer

Given `head` which is a reference node to a singly-linked list.</br> 
The value of each node in the linked list is either 0 or 1.</br> 
The linked list holds the binary representation of a number.

Return the *decimal value* of the number in the linked list.


__Example 1:__
```
Input: head = [1,0,1]
Output: 5
Explanation: (101) in base 2 = (5) in base 10
```
__Example 2:__
```
Input: head = [0]
Output: 0
```
__Example 3:__
```
Input: head = [1]
Output: 1
```
__Example 4:__
```
Input: head = [1,0,0,1,0,0,1,1,1,0,0,0,0,0,0]
Output: 18880
```
__Example 5:__
```
Input: head = [0,0]
Output: 0
```

__Constraints:__
* The Linked List is not empty.
* Number of nodes will not exceed `30`.
* Each node's value is either `0` or `1`.

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
    func getDecimalValue(_ head: ListNode?) -> Int {
        var head = head, result = 0
        while let curr = head {
            result <<= 1
            result |= curr.val
            head = curr.next
        }
        return result
    }
}
```