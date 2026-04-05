
### Maximum Binary Tree

Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:

1. The root is the maximum number in the array.
2. The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.
3. The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.

Construct the maximum tree by the given array and output the root node of this tree.

__Example 1:__
```
Input: [3,2,1,6,0,5]
Output: return the tree root node representing the following tree:

      6
    /   \
   3     5
    \    / 
     2  0   
       \
        1
```

__Note:__
* The size of the given array will be in the range `[1,1000]`.

### Solution
__O(nums * log(nums)~pow(nums, 2)) Time:__
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
    func constructMaximumBinaryTree(_ nums: [Int]) -> TreeNode? {
        build(nums, ClosedRange(uncheckedBounds: (0, nums.count - 1)))
    }

    func build(_ nums: [Int], _ range: ClosedRange<Int>) -> TreeNode? {
        if range.lowerBound > range.upperBound {
            return nil
        } else if range.lowerBound == range.upperBound {
            return TreeNode(nums[range.lowerBound])
        } else {
            var maxIndex: Int = range.lowerBound
            for i in range where nums[i] > nums[maxIndex] {
                maxIndex = i
            }
            let node: TreeNode = TreeNode(nums[maxIndex])
            node.left = build(nums, ClosedRange(uncheckedBounds: (range.lowerBound, maxIndex - 1)))
            node.right = build(nums, ClosedRange(uncheckedBounds: (maxIndex + 1, range.upperBound)))
            return node
        }
    }
}
```