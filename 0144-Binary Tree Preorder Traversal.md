
### Binary Tree Preorder Traversal

Given a binary tree, return the preorder traversal of its nodes' values.

__Example:__
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]
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
    func preorderTraversal(_ root: TreeNode?) -> [Int] {
        var result : [Int] = []
        preOrder(root, &result)
        return result
    }
    
    func preOrder(_ node: TreeNode?, _ result: inout [Int]) {
        guard let node = node else { return }
        result.append(node.val)
        preOrder(node.left, &result)
        preOrder(node.right, &result)
    }
}
```
__Iterative O(n):__
```Swift
class Solution {
    func preorderTraversal(_ root: TreeNode?) -> [Int] {
        guard let root = root else { return [] }
        var stack: [TreeNode] = [root], result : [Int] = []
        while !stack.isEmpty {
            let size = stack.count
            for _ in 0..<size {
                let node = stack.removeLast()
                if let right = node.right {
                    stack.append(right)
                }
                if let left = node.left {
                    stack.append(left)
                }
                result.append(node.val)
            }
        }
        return result
    }
}
```
```