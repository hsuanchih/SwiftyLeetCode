
### Unique Binary Search Trees

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:
* The left subtree of a node contains only nodes with keys __less than__ the node's key.
* The right subtree of a node contains only nodes with keys __greater than__ the node's key.
* Both the left and right subtrees must also be binary search trees.

__Example 1:__
```
    2
   / \
  1   3

Input: [2,1,3]
Output: true
```
__Example 2:__
```
    5
   / \
  1   4
     / \
    3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

### Solution
__Recursive:__
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
    func isValidBST(_ root: TreeNode?) -> Bool {
        return isValidBST(root, Int.min, Int.max)
    }
    
    func isValidBST(_ node: TreeNode?, _ min: Int, _ max: Int) -> Bool {
        guard let node = node else { return true }
        switch node.val {
            case (min+1..<max):
            return isValidBST(node.left, min, node.val) && isValidBST(node.right, node.val, max)
            default:
            return false
        }
    }
}
```
__Iterative:__
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
    func isValidBST(_ root: TreeNode?) -> Bool {
        var result : [Int] = []
        return inOrder(root, &result)
    }

    func inOrder(_ node: TreeNode?, _ result: inout [Int]) -> Bool {
        guard let node = node else {
            return true
        }
        let left = inOrder(node.left, &result)
        if !result.isEmpty {
            if result.removeLast() >= node.val {
                return false
            }
        }
        result.append(node.val)
        return left && inOrder(node.right, &result)
    }
}
```