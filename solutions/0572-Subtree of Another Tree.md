
### Subtree of Another Tree

Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.

__Example 1:__
```
Given tree s:

     3
    / \
   4   5
  / \
 1   2
Given tree t:
   4 
  / \
 1   2
Return true, because t has the same structure and node values with a subtree of s.
```
__Example 2:__
```
Given tree s:

     3
    / \
   4   5
  / \
 1   2
    /
   0
Given tree t:
   4
  / \
 1   2
Return false.
```

### Solution
__O(s*t) Time - Recursive:__
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
    func isSubtree(_ s: TreeNode?, _ t: TreeNode?) -> Bool {
        if isSameTree(s, t) {
            return true
        }
        if let left = s?.left, isSubtree(left, t) {
            return true
        }
        if let right = s?.right, isSubtree(right, t) {
            return true
        }
        return false
    }
    
    func isSameTree(_ s: TreeNode?, _ t: TreeNode?) -> Bool {
        switch (s?.val, t?.val) {
            case (.none, .none):
            return true
            case let (.some(sVal), .some(tVal)) where sVal == tVal:
            return isSameTree(s?.left, t?.left) && isSameTree(s?.right, t?.right)
            default:
            return false
        }
    }
}
```