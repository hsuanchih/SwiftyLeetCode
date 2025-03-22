
### Binary Tree Paths

Given a binary tree, return all root-to-leaf paths.

__Note:__ A leaf is a node with no children.

__Example:__
```
Input:

   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3
```

### Solution
__Recursive - Top Down:__
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
    func binaryTreePaths(_ root: TreeNode?) -> [String] {
        guard let root else { return [] }
        var result: [String] = []
        path(root, "", &result)
        return result
    }

    func path(_ node: TreeNode, _ temp: String, _ result: inout [String]) {
        let temp: String = temp + "\(node.val)"
        if node.left == nil, node.right == nil {
            result.append(temp)
        } else {
            if let left = node.left {
                path(left, temp + "->", &result)
            }
            if let right = node.right {
                path(right, temp + "->", &result)
            }
        }
    }
}
```
__Recursive - Bottom Up:__
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
    func binaryTreePaths(_ root: TreeNode?) -> [String] {
        guard let root else { return [] }
        if root.left == nil, root.right == nil {
            return ["\(root.val)"]
        } else {
            var paths: [String] = binaryTreePaths(root.left) + binaryTreePaths(root.right)
            for i in 0 ..< paths.count {
                paths[i] = "\(root.val)->\(paths[i])"
            }
            return paths
        }
    }
}
```