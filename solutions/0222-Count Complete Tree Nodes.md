
### Count Complete Tree Nodes

Given a __complete__ binary tree, count the number of nodes.

__Note:__

__Definition of a complete binary tree from Wikipedia:__
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

__Example:__
```
Input: 
    1
   / \
  2   3
 / \  /
4  5 6

Output: 6
```

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
    func countNodes(_ root: TreeNode?) -> Int {
        guard let node = root else { return 0 }
        return 1 + countNodes(node.left) + countNodes(node.right)
    }
}
```