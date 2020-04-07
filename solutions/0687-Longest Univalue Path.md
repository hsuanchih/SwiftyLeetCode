
### Longest Univalue Path

Given a binary tree, find the length of the longest path where each node in the path has the same value. This path may or may not pass through the root.

The length of path between two nodes is represented by the number of edges between them.

__Example 1:__
```
Input:

              5
             / \
            4   5
           / \   \
          1   1   5
Output: 2
```
__Example 2:__

Input:
```
              1
             / \
            4   5
           / \   \
          4   4   5
Output: 2
```

__Note:__ The given binary tree has not more than `10000` nodes. The height of the tree is not more than `1000`.

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
    func longestUnivaluePath(_ root: TreeNode?) -> Int {
        var longest = 0
        solve(root, &longest)
        return longest
    }
    
    func solve(_ node: TreeNode?, _ longest: inout Int) -> Int {
        guard let node = node else { return 0 }
        let left = solve(node.left, &longest), right = solve(node.right, &longest)
        switch (node.left?.val, node.right?.val) {
            case (.some(node.val), .some(node.val)):
            longest = max(longest, 2+left+right)
            return 1+max(left, right)
            case (.some(node.val), _):
            longest = max(longest, 1+left)
            return 1+left
            case (_, .some(node.val)):
            longest = max(longest, 1+right)
            return 1+right
            default:
            return 0
        }
    }
}
```