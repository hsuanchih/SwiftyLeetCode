
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
__O(n):__
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
        
        // globalMax tracks the global maximum of all single-path, two-path, and single node
        // sum of the binary tree
        var globalMax = Int.min
        let singlePathMax = maxPathSum(root, &globalMax)
        
        // Return the maximum of the single path sum/global max leading up to root
        return max(globalMax, singlePathMax)
    }
    
    func maxPathSum(_ node: TreeNode?, _ globalMax: inout Int) -> Int {
        guard let node = node else { return 0 }
        let left = maxPathSum(node.left, &globalMax), right = maxPathSum(node.right, &globalMax)
        
        // The global max will be the maximum of single path max from either the left & right sub-tree,
        // the two path max from both the left & right sub-trees meeting at node, or just the node
        // itself if the two options are negative
        let singlePathMax = max(left, right), twoPathMax = left + right
        globalMax = max(globalMax, max(max(0, singlePathMax), twoPathMax) + node.val)
        
        // Return the maximum single path sum going through the current node, if the single path sum
        // up to the current node is negative, we want to include the current node only as the path
        // leading up to the current node will only yield a smaller result
        return max(singlePathMax, 0) + node.val
    }
}
```