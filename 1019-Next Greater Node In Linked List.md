
### Best Sightseeing Pair

We are given a linked list with `head` as the first node.</br>
Let's number the nodes in the list: `node_1`, `node_2`, `node_3`, ... etc.

Each node may have a *next larger* __value__:</br> 
for `node_i`, `next_larger(node_i)` is the `node_j.val` such that `j > i`, `node_j.val > node_i.val`,</br> 
and `j` is the smallest possible choice.  If such a `j` does not exist, the next larger value is `0`.

Return an array of integers `answer`, where `answer[i] = next_larger(node_{i+1})`.

Note that in the example __inputs__ (not outputs) below, arrays such as `[2,1,5]` represent the serialization</br> 
of a linked list with a head node value of 2, second node value of 1, and third node value of 5.

__Example 1:__
```
Input: [2,1,5]
Output: [5,5,0]
```
__Example 2:__
```
Input: [2,7,4,3,5]
Output: [7,0,5,5,0]
```
__Example 3:__
```
Input: [1,7,5,1,9,2,5,1]
Output: [7,9,9,9,0,5,0,0]
```

__Note:__
1. `1 <= node.val <= 10^9` for each node in the linked list.
2. The given list has length in the range `[0, 10000]`.

### Solution
__O(n^2) Time, O(1) Space:__
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
    func nextLargerNodes(_ head: ListNode?) -> [Int] {
        var nums = serialize(head)

        // For each element in nums, identify the next greater element
        for i in 0..<nums.count {

            // The last element will not have a next greater element
            if i == nums.count-1 {
                nums[i] = 0
                break
            }

            // Traverse through elements after element at i
            for j in i+1..<nums.count {

                // If element is found, assign its value to nums[i]
                if nums[j] > nums[i] {
                    nums[i] = nums[j]
                    break
                }

                // No element greater than element at i found
                if j == nums.count-1 {
                    nums[i] = 0
                }
            }
        }
        return nums
    }

    func serialize(_ head: ListNode?) -> [Int] {
        var head = head, result : [Int] = []
        while let curr = head {
            result.append(curr.val)
            head = curr.next
        }
        return result
    }
}
```
__O(n) Time, O(n) Space:__
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
    func nextLargerNodes(_ head: ListNode?) -> [Int] {
        var nums = serialize(head)

        // Use a stack to track the next greater element as nums
        // is traversed from the end
        var stack : [Int] = []
        for i in stride(from: nums.count-1, through: 0, by: -1) {
            let num = nums[i]

            // Keep popping the stack until element at the top of the stack
            // is greater than num
            while let last = stack.last, last <= num {
                _ = stack.removeLast()
            }

            // If stack is empty, there are no elements greater than num
            // after num's index (assign 0), otherwise the top element is
            // the first greater element.
            if let last = stack.last {
                nums[i] = last
            } else {
                nums[i] = 0
            }

            // Push element onto the stack
            stack.append(num)
        }
        return nums
    }

    func serialize(_ head: ListNode?) -> [Int] {
        var head = head, result : [Int] = []
        while let curr = head {
            result.append(curr.val)
            head = curr.next
        }
        return result
    }
}
```