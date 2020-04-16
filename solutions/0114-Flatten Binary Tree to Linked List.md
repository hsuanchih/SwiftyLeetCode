
### Flatten Binary Tree to Linked List

Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:
```
    1
   / \
  2   5
 / \   \
3   4   6
```
The flattened tree should look like:
```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

### Solution
__O(2*n) Time - Post-Order:__
```Swift
class Solution {
    func flatten(_ root: TreeNode?) {
        solve(root)
    }
    
    func solve(_ node: TreeNode?) -> TreeNode? {
        guard let node = node else { return nil }
        let left = solve(node.left), right = solve(node.right)
        node.left = nil
        node.right = left
        var curr : TreeNode? = node
        while curr?.right != nil {
            curr = curr?.right
        }
        curr?.right = right
        return node
    }
}
```
__O(n) Time - Reverse Post-Order:__
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
    func flatten(_ root: TreeNode?) {
        var head : TreeNode?
        reversePostOrder(root, &head)
    }
    
    // Traverse the binary tree in reverse Post-Order traversal
    // ie, process in the order of right sub-tree, then left-subtree, then root
    func reversePostOrder(_ node: TreeNode?, _ head: inout TreeNode?) {
        guard let node = node else { return }
        reversePostOrder(node.right, &head)
        reversePostOrder(node.left, &head)
        node.left = nil
        node.right = head
        head = node
    }
}
```
__O(n) Time - Pre-Order:__
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
    func flatten(_ root: TreeNode?) {
        var curr : TreeNode? = TreeNode(0)
        preOrder(root, &curr)
    }
    
    func preOrder(_ node: TreeNode?, _ curr: inout TreeNode?) {
        guard let node = node else { return }
        curr?.right = node
        curr?.left = nil
        curr = curr?.right
        let right = node.right
        preOrder(node.left, &curr)
        preOrder(right, &curr)
    }
}
```