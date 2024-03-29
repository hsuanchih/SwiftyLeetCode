
### Binary Tree Inorder Traversal

Given a binary tree, return the inorder traversal of its nodes' values.

__Example:__
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```
__Follow up:__ Recursive solution is trivial, could you do it iteratively?

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
    func inorderTraversal(_ root: TreeNode?) -> [Int] {
        var result: [Int] = []
        inOrder(node: root, result: &result)
        return result
    }

    func inOrder(node: TreeNode?, result: inout [Int]) {
        guard let node = node else { return }
        inOrder(node: node.left, result: &result)
        result.append(node.val)
        inOrder(node: node.right, result: &result)
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
    func inorderTraversal(_ root: TreeNode?) -> [Int] {
        var stack : [TreeNode] = []
        var result : [Int] = []
        var curr = root

        // Terminate when current is nil or stack is empty
        while curr != nil || !stack.isEmpty {
            // For a given node, push itself & all its left children
            // onto the stack
            while let node = curr {
                stack.append(node)
                curr = node.left
            }
            // The last item on the stack is the left-most child of the
            // current node (that does not have a left child)
            // Add it to result & set its right child as the next node
            // to repeat the same process
            let node = stack.removeLast()
            result.append(node.val)
            curr = node.right
        }
        return result
    }
}
```