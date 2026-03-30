
### Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

__Note:__ A leaf is a node with no children.

__Example:__
Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its minimum depth = 2.

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
    func minDepth(_ root: TreeNode?) -> Int {
        guard let root else { return 0 }
        return depth(root)
    }

    func depth(_ node: TreeNode) -> Int {
        let height: Int
        switch (node.left, node.right) {
        case (.none, .none):
            height = 0
        case (.some(let left), .none):
            height = depth(left)
        case (.none, .some(let right)):
            height = depth(right)
        case (.some(let left), .some(let right)):
            height = min(depth(left), depth(right))
        }
        return height + 1
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
    func minDepth(_ root: TreeNode?) -> Int {
        guard let root else { return 0 }
        var queue: [TreeNode] = [root]
        var depth: Int = 0
        while !queue.isEmpty {
            let count: Int = queue.count
            depth += 1
            for _ in 0 ..< count {
                let node = queue.removeFirst()
                switch (node.left, node.right) {
                case (.none, .none):
                    return depth
                case (.some(let left), .none):
                    queue.append(left)
                case (.none, .some(let right)):
                    queue.append(right)
                case (.some(let left), .some(let right)):
                    queue.append(left)
                    queue.append(right)
                }
            }
        }
        return depth
    }
}
```