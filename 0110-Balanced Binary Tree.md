
### Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.
For this problem, a height-balanced binary tree is defined as:
```
a binary tree in which the left and right subtrees of every node differ in height by no more than 1.
```

__Example 1:__
Given the following tree `[3,9,20,null,null,15,7]`:
```
    3
   / \
  9  20
    /  \
   15   7
```
Return true.

__Example 2:__
Given the following tree `[1,2,2,3,3,null,null,4,4]`:
```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```
Return false.

### Solution
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
    func isBalanced(_ root: TreeNode?) -> Bool {
        guard let node = root else { return true }
        return abs(maxHeight(node.left) - maxHeight(node.right)) <= 1 && isBalanced(node.left) && isBalanced(node.right)
    }
    
    func maxHeight(_ node: TreeNode?) -> Int {
        guard let node = node else { return 0 }
        return max(maxHeight(node.left), maxHeight(node.right)) + 1
    }
}
```