
### Sum of Left Leaves

Find the sum of all left leaves in a given binary tree.

__Example:__
```
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
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
    func sumOfLeftLeaves(_ root: TreeNode?) -> Int {
        return sum(root?.left, true) + sum(root?.right, false)
    }
    
    func sum(_ node: TreeNode?, _ isLeft: Bool) -> Int {
        guard let node = node else { return 0 }
        switch (node.left, node.right) {
            case (.none, .none):
            return isLeft ? node.val : 0
            default:
            return sum(node.left, true) + sum(node.right, false)
        }
    }
}
```