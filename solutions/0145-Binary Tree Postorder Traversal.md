
### Binary Tree Preorder Traversal

Given a binary tree, return the postorder traversal of its nodes' values.

__Example:__
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [3,2,1]
```

__Follow up:__ Recursive solution is trivial, could you do it iteratively?

### Solution
__Recursive O(n):__
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
    func postorderTraversal(_ root: TreeNode?) -> [Int] {
        var result : [Int] = []
        postOrder(root, &result)
        return result
    }
    
    func postOrder(_ node: TreeNode?, _ result: inout [Int]) {
        guard let node = node else {
            return
        }
        postOrder(node.left, &result)
        postOrder(node.right, &result)
        result.append(node.val)
    }
}
```
__Iterative O(n):__
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
    func postorderTraversal(_ root: TreeNode?) -> [Int] {
        var stack : [TreeNode] = [], result : [Int] = []
        var curr = root
        while curr != nil || !stack.isEmpty {
            while let node = curr {
                if let right = node.right {
                    stack.append(right)
                }
                stack.append(node)
                curr = node.left
            }
            
            let node = stack.removeLast()
            
            if let right = node.right, right === stack.last {
                _ = stack.removeLast()
                stack.append(node)
                curr = right
            } else {
                result.append(node.val)
            }
        }
        return result
    }
}
```