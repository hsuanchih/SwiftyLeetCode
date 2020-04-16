
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
__O(n*log\[\base 2](n)~n^2) Time:__
```Swift
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 * }
 */
class Solution {
    func constructMaximumBinaryTree(_ nums: [Int]) -> TreeNode? {
        return construct(nums, 0, nums.count-1)
    }
    
    func construct(_ nums: [Int], _ start: Int, _ end: Int) -> TreeNode? {

        // Base case
        if start > end {
            return nil
        }

        // Find the index of the largest item in the array
        var maxIndex = start
        for i in start...end {
            if nums[i] > nums[maxIndex] {
                maxIndex = i
            }
        }

        // Construct tree node from element
        // Left subtree consists of elements before the largest element
        // Right subtree consists of elements after the largest element
        let curr = TreeNode(nums[maxIndex])
        curr.left = construct(nums, start, maxIndex-1)
        curr.right = construct(nums, maxIndex+1, end)
        return curr
    }
}
```