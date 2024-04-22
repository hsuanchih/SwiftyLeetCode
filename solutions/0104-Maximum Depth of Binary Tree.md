
### Maximum Depth of Binary Tree

Given the `root` of a binary tree, return its maximum depth.

A binary tree's __maximum depth__ is the number of nodes along the longest path from the root node down to the farthest leaf node.

__Example 1:__

![question_104.jpg](../images/question_104.jpg)
```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```
__Example 2:__
```
Input: root = [1,null,2]
Output: 2
```

__Constraints:__
* The number of nodes in the tree is in the range `[0, 104]`.
* `-100 <= Node.val <= 100`

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
    func maxDepth(_ root: TreeNode?) -> Int {
        guard let root else { return 0 }
        return max(maxDepth(root.right), maxDepth(root.left)) + 1
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
    func maxDepth(_ root: TreeNode?) -> Int {
        guard let root else { return 0 }
        var depth: Int = 0
        var queue: [TreeNode] = [root]
        while !queue.isEmpty {
            depth += 1
            let count: Int = queue.count
            for _ in 0 ..< count {
                let node: TreeNode = queue.removeFirst()
                [node.left, node.right].forEach {
                    guard let next = $0 else { return }
                    queue.append(next)
                }
            }
        }
        return depth
    }
}
```