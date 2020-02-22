
### Univalued Binary Tree

A binary tree is *univalued* if every node in the tree has the same value.

Return `true` if and only if the given tree is univalued.

__Example 1:__

![Example 1](images/question_965-0.png)
```
Input: [1,1,1,1,1,null,1]
Output: true
```
__Example 2:__

![Example 2](images/question_965-1.png)
```
Input: [2,2,2,5,2]
Output: false
```

__Note:__
1. The number of nodes in the given tree will be in the range `[1, 100]`.
2. Each node's value will be an integer in the range `[0, 99]`.

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
    func isUnivalTree(_ root: TreeNode?) -> Bool {
        guard let root = root else { return false }
        return univalue(root, root.val)
    }
    
    func univalue(_ node: TreeNode?, _ value: Int) -> Bool {
        guard let node = node else { return true }
        if node.val != value {
            return false
        }
        return univalue(node.left, value) && univalue(node.right, value)
    }
}
```
