
### Path Sum II

Given the `root` of a binary tree and an integer `targetSum`, return all __root-to-leaf__ paths where the sum of the node values in the path equals `targetSum`. Each path should be returned as a list of the node values, not node references.

A __root-to-leaf__ path is a path starting from the root and ending at any leaf node. A __leaf__ is a node with no children.

__Example 1:__

![question_113-0.jpg](../images/question_113-0.jpg)
```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
Explanation: There are two paths whose sum equals targetSum:
5 + 4 + 11 + 2 = 22
5 + 8 + 4 + 5 = 22
```
__Example 2:__

![question_113-1.jpg](../images/question_113-1.jpg)
```
Input: root = [1,2,3], targetSum = 5
Output: []
```
__Example 3:__
```
Input: root = [1,2], targetSum = 0
Output: []
```

__Constraints:__
* The number of nodes in the tree is in the range `[0, 5000]`.
* `-1000 <= Node.val <= 1000`
* `-1000 <= targetSum <= 1000`

### Solution
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
    func pathSum(_ root: TreeNode?, _ targetSum: Int) -> [[Int]] {
        var result: [[Int]] = []
        traverse(root, targetSum, [], &result)
        return result
    }

    func traverse(_ node: TreeNode?, _ targetSum: Int, _ values: [Int], _ result: inout [[Int]]) {
        guard let node else { return }
        let targetSum: Int = targetSum - node.val
        let values: [Int] = values + [node.val]
        if node.left == nil, node.right == nil {
            if targetSum == 0 {
                result.append(values)
            }
        } else {
            [node.left, node.right].forEach {
                traverse($0, targetSum, values, &result)
            }
        }
    }
}
```