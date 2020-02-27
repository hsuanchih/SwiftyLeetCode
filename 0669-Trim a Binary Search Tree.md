
### Trim a Binary Search Tree

Given a binary search tree and the lowest and highest boundaries as `L` and `R`,</br> 
trim the tree so that all its elements lies in `[L, R]` (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.

__Example 1:__
```
Input: 
    1
   / \
  0   2

  L = 1
  R = 2

Output: 
    1
      \
       2
```
__Example 2:__
```
Input: 
    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3

Output: 
      3
     / 
   2   
  /
 1
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
    func trimBST(_ root: TreeNode?, _ L: Int, _ R: Int) -> TreeNode? {
        guard let node = root else { return nil }
        switch node.val {
            case Int.min..<L:
            return trimBST(node.right, L, R)
            case R+1...Int.max:
            return trimBST(node.left, L, R)
            default:
            node.left = trimBST(node.left, L, R)
            node.right = trimBST(node.right, L, R)
            return node
        }
    }
}
```