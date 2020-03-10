
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
__O(2*n):__
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
    func flatten(_ root: TreeNode?) {
        var head : TreeNode?
        reversePreOrder(root, &head)
    }
    
    // Traverse the binary tree in reverse Post-Order traversal
    // ie, process in the order of right sub-tree, then left-subtree, then root
    func reversePreOrder(_ node: TreeNode?, _ head: inout TreeNode?) {
        guard let node = node else { return }
        reversePreOrder(node.right, &head)
        reversePreOrder(node.left, &head)
        node.left = nil
        node.right = head
        head = node
    }
}
```