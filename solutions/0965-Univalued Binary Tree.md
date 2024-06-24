
### Univalued Binary Tree

A binary tree is *univalued* if every node in the tree has the same value.

Return `true` if and only if the given tree is univalued.

__Example 1:__

![Example 1](images/question_965-0.png)
```
Input: [1,1,1,1,1,null,1]
Output: true
```
__Example 2:__

![Example 2](images/question_965-1.png)
```
Input: [2,2,2,5,2]
Output: false
```

__Note:__
1. The number of nodes in the given tree will be in the range `[1, 100]`.
2. Each node's value will be an integer in the range `[0, 99]`.

### Solution
__Recursive:__
```Swift
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init() { self.val = 0; self.left = nil; self.right = nil; }
 *     public init(_ val: Int) { self.val = val; self.left = nil; self.right = nil; }
 *     public init(_ val: Int, _ left: TreeNode?, _ right: TreeNode?) {
 *         self.val = val
 *         self.left = left
 *         self.right = right
 *     }
 * }
 */
class Solution {
    func isUnivalTree(_ root: TreeNode?) -> Bool {
        guard let root else { return true }
        if let left = root.left, root.val != left.val {
            return false
        }
        if let right = root.right, root.val != right.val {
            return false
        }
        return isUnivalTree(root.left) && isUnivalTree(root.right)
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
 *     public init() { self.val = 0; self.left = nil; self.right = nil; }
 *     public init(_ val: Int) { self.val = val; self.left = nil; self.right = nil; }
 *     public init(_ val: Int, _ left: TreeNode?, _ right: TreeNode?) {
 *         self.val = val
 *         self.left = left
 *         self.right = right
 *     }
 * }
 */
class Solution {
    func isUnivalTree(_ root: TreeNode?) -> Bool {
        guard let root else { return true }
        var stack: [TreeNode] = [root]
        while !stack.isEmpty {
            let node: TreeNode = stack.removeLast()
            if node.val != root.val {
                return false
            } else {
                if let left = node.left {
                    stack.append(left)
                }
                if let right = node.right {
                    stack.append(right)
                }
            }
        }
        return true
    }
}
```