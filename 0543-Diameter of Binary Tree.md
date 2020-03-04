
### Diameter of Binary Tree

Given a binary tree, you need to compute the length of the diameter of the tree.</br> 
The diameter of a binary tree is the length of the __longest__ path between any two nodes in a tree.</br> 
This path may or may not pass through the root.

__Example:__
```
Given a binary tree
          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
```

__Note:__ The length of path between two nodes is represented by the number of edges between them.

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
    func diameterOfBinaryTree(_ root: TreeNode?) -> Int {
        var result = 1
        _ = solve(root, &result)
        return result-1
    }
    
    func solve(_ node: TreeNode?, _ result: inout Int) -> Int {
        guard let node = node else { return 0 }
        let left = solve(node.left, &result), right = solve(node.right, &result)
        result = max(result, left+right+1)
        return max(left, right)+1
    }
}
```