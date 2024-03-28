
### Convert Sorted Array to Binary Search Tree

Given an integer array `nums` where the elements are sorted in __ascending order__, convert it to a height-balanced binary search tree.

__Example 1:__
![images/question_108-0.jpg](../images/question_108-0.jpg)
```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:
```
![images/question_108-1.jpg](../images/question_108-1.jpg)
__Example 2:__
![images/question_108-2.jpg](../images/question_108-2.jpg)
```
Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.
```
 
__Constraints:__
* `1 <= nums.length <= pow(10, 4)`
* `-pow(10, 4) <= nums[i] <= pow(10, 4)`
* `nums` is sorted in a strictly increasing order.

### Solution
```Swift
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init() { self.val = 0; self.left = nil; self.right = nil; }
 *     public init(_ val: Int) { self.val = val; self.left = nil; self.right = nil; }
 *     public init(_ val: Int, _ left: TreeNode?, _ right: TreeNode?) {
 *         self.val = val
 *         self.left = left
 *         self.right = right
 *     }
 * }
 */
class Solution {
    func sortedArrayToBST(_ nums: [Int]) -> TreeNode? {
        convert(nums, ClosedRange(uncheckedBounds: (0, nums.count - 1)))
    }

    func convert(_ nums: [Int], _ range: ClosedRange<Int>) -> TreeNode? {
        if range.lowerBound > range.upperBound {
            return nil
        } else {
            let mid: Int = range.lowerBound + (range.upperBound - range.lowerBound) / 2
            let node: TreeNode = TreeNode(nums[mid])
            node.left = convert(nums, ClosedRange(uncheckedBounds: (range.lowerBound, mid - 1)))
            node.right = convert(nums, ClosedRange(uncheckedBounds: (mid + 1, range.upperBound)))
            return node 
        }
    }
}
```