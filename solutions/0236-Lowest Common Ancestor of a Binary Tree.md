
### Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we __allow a node to be a descendant of itself__).”

__Example 1:__

![question_236-0.png](../images/question_236-0.png)
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```
__Example 2:__

![question_236-1.png](../images/question_236-1.png)
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```
__Example 3:__
```
Input: root = [1,2], p = 1, q = 2
Output: 1
``` 

__Constraints:__
* The number of nodes in the tree is in the range `[2, pow(10, 5)]`.
* `-pow(10, 9) <= Node.val <= pow(10, 9)`
* All Node.val are __unique__.
* `p != q`
* `p` and `q` will exist in the tree.

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
        guard let root else { return nil }
        let left: TreeNode? = lowestCommonAncestor(root.left, p, q)
        let right: TreeNode? = lowestCommonAncestor(root.right, p, q)
        switch (left, right) {
        case (.some, .some):
            return root
        case (.none, .some(let right)):
            return root === p || root === q ? root : right
        case (.some(let left), .none):
            return root === p || root === q ? root : left
        case (.none, .none):
            return root === p || root === q ? root : nil
        }
    }
}
```
__O(n) Recursive - Optimized Pruning:__
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
        guard let root else { return nil }
        if root === p || root === q {
            // The assumption here is that p & q will both exist in the binary tree, and there exists exactly one LCA.
            // If the current node's value matches either p or q, then the matching node must be a potential LCA 
            // unless found otherwise (that the other matching node does not exist in the current node's subtrees), 
            // in which case the LCA will be re-assigned when the complimentary subtree traversal returns.
            return root
        } else {
            switch (lowestCommonAncestor(root.left, p, q), lowestCommonAncestor(root.right, p, q)) {
            case (.some, .some):
                // If both the current node's left & right subtrees contain p and q, then the node must be the lowest
                // common ancestor.
                return root
            case (let left, let right):
                // Either p or q is found in one of the subtrees of the current node, or neither is found:
                // propagate the result upwards.
                return left ?? right
            }
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
        
        // Use `stack` for level-order traversal, and `parent` to track the parent of each tree node
        var stack : [TreeNode] = [root], parent : [TreeNode:TreeNode] = [:]
        
        // We want to early return if both p & q have been visited  (direct parents of p & q have been found)
        // Otherwise, level-traverse through the entire tree.
        while (parent[p!] == nil || parent[q!] == nil) && !stack.isEmpty {
        
            // Pop the most recently added node on from the stack, 
            // and record this node as the parent of its left & right children
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
        
        // Start with node `p` and unwind its anscestors from one parent to another, 
        // adding each ancestor to the `parents` set.
        var parents : Set<TreeNode> = [], p = p
        while let curr = p {
            parents.insert(curr)
            p = parent[curr]
        }
        
        // Now that `p`'s parents have been added to the `parents` set, 
        // unwind node `q`'s ancestors from one parent to another.
        // The first ancestor unwound from `q` to match an ancestor of `p` will be the LCA.
        var q = q
        while let curr = q, !parents.contains(curr) {
            q = parent[curr]
        }
        return q
    }
}
```
