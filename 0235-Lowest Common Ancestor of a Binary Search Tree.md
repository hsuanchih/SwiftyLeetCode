
### Lowest Common Ancestor of a Binary Search Tree

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia:</br> 
“The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants</br> 
(where we allow __a node to be a descendant of itself__).”

Given binary search tree:  root = [6,2,8,0,4,7,9,null,null,3,5]


__Example 1:__
```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```
__Example 2:__
```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

__Note:__
* All of the nodes' values will be unique.
* p and q are different and both values will exist in the BST.

### Solution
__O(log\[base2\](n)) Recursive:__
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
        switch (p!.val, q!.val) {
            case (Int.min..<node.val, Int.min..<node.val):
            return lowestCommonAncestor(node.left, p, q)
            case (node.val+1...Int.max, node.val+1...Int.max):
            return lowestCommonAncestor(node.right, p, q)
            default:
            return node
        }
    }
}
```
__O(log\[base2\](n)) Iterative:__
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
        var curr = root
        while let node = curr {
            switch (p!.val, q!.val) {
                case (Int.min..<node.val, Int.min..<node.val):
                curr = node.left
                case (node.val+1...Int.max, node.val+1...Int.max):
                curr = node.right
                default:
                return node
            }
        }
        return nil
    }
}
```