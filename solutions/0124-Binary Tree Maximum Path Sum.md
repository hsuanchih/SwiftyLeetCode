
### Binary Tree Maximum Path Sum

Given a __non-empty__ binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain __at least one node__ and does not need to go through the root.

__Example 1:__
```
Input: [1,2,3]

       1
      / \
     2   3

Output: 6
```
__Example 2:__
```
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```

### Solution
__O(n) Time - Recursive:__
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
    func maxPathSum(_ root: TreeNode?) -> Int {
        var result = Int.min
        maxPathSum(root, &result)
        return result
    }
    
    func maxPathSum(_ node: TreeNode?, _ result: inout Int) -> Int {
        guard let node = node else { return 0 }
        let leftSum = max(0, maxPathSum(node.left, &result)), rightSum = max(0, maxPathSum(node.right, &result))
        result = max(result, node.val+leftSum+rightSum)
        return node.val+max(leftSum, rightSum)
    }
}
```