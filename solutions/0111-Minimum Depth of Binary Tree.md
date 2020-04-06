
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
__O(n) Time, O(1) Space - Recursive:__
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
    func minDepth(_ root: TreeNode?) -> Int {
        guard let node = root else { return 0 }
        var curr = 0
        switch (node.left, node.right) {
            case (.some(_), .none):
            curr = minDepth(node.left)
            case (.none, .some(_)):
            curr = minDepth(node.right)
            default:
            curr = min(minDepth(node.left), minDepth(node.right))
        }
        return curr + 1
    }
}
```
__O(n) Time, O(n/2) Space - Iterative:__
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
    func minDepth(_ root: TreeNode?) -> Int {
        guard let root = root else { return 0 }
        var queue = [root], depth = 0
        while !queue.isEmpty {
            let length = queue.count
            depth += 1
            for _ in 0..<length {
                let curr = queue.removeFirst()
                switch (curr.left, curr.right) {
                    case (.some(let left), .none):
                    queue.append(left)
                    case (.none, .some(let right)):
                    queue.append(right)
                    case let (.some(left), .some(right)):
                    queue.append(left)
                    queue.append(right)
                    default:
                    return depth
                }
            }
        }
        return depth
    }
}
```