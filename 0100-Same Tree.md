
### Same Tree

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

__Example 1:__
```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true
```
__Example 2:__
```
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false
```
__Example 3:__
```
Input:     1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

Output: false
```

### Solution
__Recursive:__
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
    func isSameTree(_ left: TreeNode?, _ right: TreeNode?) -> Bool {
        switch (left, right) {
            case let (.some(left), .some(right)):
            return left.val == right.val && isSameTree(left.left, right.left) && isSameTree(left.right, right.right)
            case (.none, .none):
            return true
            default:
            return false
        }
    }
}
```