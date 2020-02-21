
### Sum of Root To Leaf Binary Numbers

Given a binary tree, each node has value `0` or `1`.</br>
Each root-to-leaf path represents a binary number starting with the most significant bit.</br>
For example, if the path is `0 -> 1 -> 1 -> 0 -> 1`, then this could represent `01101` in binary, which is `13`.

For all leaves in the tree, consider the numbers represented by the path from the root to that leaf.

Return the sum of these numbers.

__Example 1:__
```
Input: [1,0,1,0,1,0,1]
Output: 22
Explanation: (100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22
```

__Note:__
1. The number of nodes in the tree is between `1` and `1000`.
2. node.val is `0` or `1`.
3. The answer will not exceed `2^31 - 1`.

### Solution
__O(n):__
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
    func sumRootToLeaf(_ root: TreeNode?) -> Int {
        var result : Int = 0
        solve(root, 0, &result)
        return result
    }
    
    func solve(_ node: TreeNode?, _ sum: Int, _ result: inout Int) {
        guard let node = node else { return }
        let sum = (sum << 1) + node.val
        switch (node.left, node.right) {
            case (.none, .none):
            result += sum
            default:
            solve(node.left, sum, &result)
            solve(node.right, sum, &result)
        }
    }
}
```