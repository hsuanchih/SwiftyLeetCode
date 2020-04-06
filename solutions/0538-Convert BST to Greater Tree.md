
### Convert BST to Greater Tree

Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

__Example:__
```
Input: The root of a Binary Search Tree like this:
              5
            /   \
           2     13

Output: The root of a Greater Tree like this:
             18
            /   \
          20     13
```
__Note:__ This question is the same as 1038: https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree/

### Solution
__O(n) Time, O(1) Space - Reverse-In-Order Traversal:__
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
    func convertBST(_ root: TreeNode?) -> TreeNode? {
        var sum = 0
        traverse(root, &sum)
        return root
    }
    
    func traverse(_ node: TreeNode?, _ sum: inout Int) {
        guard let node = node else { return }
        traverse(node.right, &sum)
        node.val += sum
        sum = node.val
        traverse(node.left, &sum)
    }
}
```