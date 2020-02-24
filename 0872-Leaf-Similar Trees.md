
### Leaf-Similar Trees

Consider all the leaves of a binary tree.  From left to right order, the values of those leaves form a *leaf value sequence*.



For example, in the given tree above, the leaf value sequence is `(6, 7, 4, 9, 8)`.

Two binary trees are considered leaf-similar if their leaf value sequence is the same.

Return `true` if and only if the two given trees with head nodes `root1` and `root2` are leaf-similar.

__Note:__
* Both of the given trees will have between `1` and `100` nodes.

### Solution
__O(n1+n2):__
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
    func leafSimilar(_ root1: TreeNode?, _ root2: TreeNode?) -> Bool {
        var result1 : [Int] = [], result2: [Int] = []
        constructLeaves(root1, &result1)
        constructLeaves(root2, &result2)
        return result1 == result2
    }
    
    func constructLeaves(_ node: TreeNode?, _ result: inout [Int]) {
        guard let node = node else { return }
        switch (node.left, node.right) {
            case (.none, .none):
            result.append(node.val)
            default:
            constructLeaves(node.left, &result)
            constructLeaves(node.right, &result)
        }
    }
}
```