
### Increasing Order Search Tree

Given a binary search tree, rearrange the tree in __in-order__ so that the leftmost node in the tree is now the root of the tree,</br> 
and every node has no left child and only 1 right child.

__Example 1:__
```
Input: [5,3,6,2,4,null,8,1,null,null,null,7,9]

       5
      / \
    3    6
   / \    \
  2   4    8
 /        / \ 
1        7   9

Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]

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
            \
             7
              \
               8
                \
                 9
```

__Note:__
1. The number of nodes in the given tree will be between 1 and 100.
2. Each node will have a unique integer value from 0 to 1000.

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
    
    // Start with an empty tree that has only right subtree (result)
    // Traverse the BST in-order, adding each node to the result tree
    func increasingBST(_ root: TreeNode?) -> TreeNode? {
        var result = TreeNode(0), curr = result
        inOrder(root, &curr)
        return result.right
    }
    
    func inOrder(_ node: TreeNode?, _ curr: inout TreeNode) {
        guard let node = node else { return }
        inOrder(node.left, &curr)
        node.left = nil
        curr.right = node
        curr = node
        inOrder(node.right, &curr)
    }
}
```