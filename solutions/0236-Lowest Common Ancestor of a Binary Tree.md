
### Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia:</br> 
“The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow __a node to be a descendant of itself__).”

Given the following binary tree:  root = [3,5,1,6,2,0,8,null,null,7,4]

![example](images/question_236.png)

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
        
        // The assumption here is that p & q will both exist in the binary tree, and there exists exactly one LCA.
        // If the current node's value matches either p or q, then the matching node must be a potential LCA 
        // unless found otherwise (that the other matching node does not exist in the current node's subtrees), 
        // in which case the LCA will be re-assigned when the complimentary subtree traversal returns.
        switch node.val {
            case p!.val: return p
            case q!.val: return q
            default: break
        }
        
        switch (lowestCommonAncestor(node.left, p, q), lowestCommonAncestor(node.right, p, q)) {
        
            // If both the current node's left & right subtrees contain p and q, then the node must be the lowest
            // common ancestor.
            case (.some, .some):
            return node
            
            // Either p or q is found in one of the subtrees of the current node, or neither is found:
            // propagate the result upwards.
            case let (left, right):
            return left ?? right
        }
    }
}
```
__O(n) Iterative - Level-Order Traversal:__
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

extension TreeNode : Hashable {
    public static func == (lhs: TreeNode, rhs: TreeNode) -> Bool {
        return lhs === rhs
    }
    public func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }
}

class Solution {
    func lowestCommonAncestor(_ root: TreeNode?, _ p: TreeNode?, _ q: TreeNode?) -> TreeNode? {
        guard let root = root else { return nil }
        var stack : [TreeNode] = [root], parent : [TreeNode:TreeNode] = [:]
        while (parent[p!] == nil || parent[q!] == nil) && !stack.isEmpty {
            let node = stack.removeLast()
            if let left = node.left {
                stack.append(left)
                parent[left] = node
            }
            if let right = node.right {
                stack.append(right)
                parent[right] = node
            }
        }
        var parents : Set<TreeNode> = [], p = p
        while let curr = p {
            parents.insert(curr)
            p = parent[curr]
        }
        
        var q = q
        while let curr = q, !parents.contains(curr) {
            q = parent[curr]
        }
        return q
    }
}
```
