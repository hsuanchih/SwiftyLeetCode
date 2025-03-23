
### Binary Tree Maximum Path Sum

A __path__ in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence __at most once__. Note that the path does not need to pass through the root.

The __path sum__ of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return the __maximum path sum__ of any __non-empty__ path.

__Example 1:__

![question_124-0.jpg](../images/question_124-0.jpg)
```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```
__Example 2:__

![question_124-1.jpg](../images/question_124-1.jpg)
```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```

__Constraints:__
* The number of nodes in the tree is in the range `[1, 3 * pow(10, 4)]`.
* `-1000 <= Node.val <= 1000`

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
    func maxPathSum(_ root: TreeNode?) -> Int {
        var maxSum: Int = .min
        _ = pathSum(root, &maxSum)
        return maxSum
    }

    func pathSum(_ node: TreeNode?, _ maxSum: inout Int) -> Int {
        guard let node else { return .min }
        let leftSum: Int = max(pathSum(node.left, &maxSum), 0)
        let rightSum: Int = max(pathSum(node.right, &maxSum), 0)
        maxSum = max(maxSum, leftSum + rightSum + node.val)
        return max(leftSum, rightSum) + node.val
    }
}
```