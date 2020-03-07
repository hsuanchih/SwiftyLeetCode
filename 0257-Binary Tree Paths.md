
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
    func binaryTreePaths(_ root: TreeNode?) -> [String] {
        var result : [String] = []
        traverse(root, "", &result)
        return result
    }
    
    func traverse(_ node: TreeNode?, _ temp: String, _ result: inout [String]) {
        guard let node = node else { return }
        let curr = temp.appending("\(node.val)")
        switch (node.left, node.right) {
        case (.none, .none):
            result.append(curr)
        default:
            [node.left, node.right].forEach {
                traverse($0, curr.appending("->"), &result)
            }
        }
    }
}
```