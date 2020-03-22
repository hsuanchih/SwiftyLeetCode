
### Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia:</br> 
“The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow __a node to be a descendant of itself__).”

Given the following binary tree:  root = [3,5,1,6,2,0,8,null,null,7,4]

__Example 1:__
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```
__Example 2:__
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

__Note:__
* All of the nodes' values will be unique.
* p and q are different and both values will exist in the BST.

### Solution
__O(n) Recursive:__
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
    func lowestCommonAncestor(_ root: TreeNode?, _ p: TreeNode?, _ q: TreeNode?) -> TreeNode? {
        guard let node = root else { return root }
        switch node.val {
            case p!.val: return p
            case q!.val: return q
            default: break
        }
        switch (lowestCommonAncestor(node.left, p, q), lowestCommonAncestor(node.right, p, q)) {
            case (.some(_), .some(_)):
            return node
            case let (left, right):
            return left ?? right
        }
    }
}
```