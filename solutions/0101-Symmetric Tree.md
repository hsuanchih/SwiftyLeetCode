
### Same Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
But the following `[1,2,2,null,3,null,3]` is not:
```
    1
   / \
  2   2
   \   \
   3    3
```

__Note:__
Bonus points if you could solve it both recursively and iteratively.

### Solution
__O(n) Time, O(1) Space - Recursive:__
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
    func isSymmetric(_ root: TreeNode?) -> Bool {
        return isMirror(root?.left, root?.right)
    }
    
    func isMirror(_ lhs: TreeNode?, _ rhs: TreeNode?) -> Bool {
        switch (lhs?.val, rhs?.val) {
            case (.none, .none):
            return true
            case let (left, right) where left == right:
            return isMirror(lhs?.left, rhs?.right) && isMirror(lhs?.right, rhs?.left)
            default:
            return false
        }
    }
}
```