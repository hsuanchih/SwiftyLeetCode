
### Merge Two Binary Trees

Given two binary trees and imagine that when you put one of them to cover the other,</br> 
some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap,</br> 
then sum node values up as the new value of the merged node.</br> 
Otherwise, the NOT null node will be used as the node of new tree.

__Example 1:__
```
Input: 
  Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
Output: 
Merged tree:
       3
      / \
     4   5
    / \   \ 
   5   4   7
```

__Note:__ The merging process must start from the root nodes of both trees.

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
    func mergeTrees(_ t1: TreeNode?, _ t2: TreeNode?) -> TreeNode? {
        switch (t1, t2) {
            case (.none, .none):
            return nil
            default:
            let curr = TreeNode((t1?.val ?? 0) + (t2?.val ?? 0))
            curr.left = mergeTrees(t1?.left, t2?.left)
            curr.right = mergeTrees(t1?.right, t2?.right)
            return curr
        }
    }
}
```