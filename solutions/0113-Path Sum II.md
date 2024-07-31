
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
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 * }
 */
class Solution {
    func pathSum(_ root: TreeNode?, _ sum: Int) -> [[Int]] {
        var temp: [Int] = [], result : [[Int]] = []
        pathSum(root, sum, &temp, &result)
        return result
    }
    
    func pathSum(_ node: TreeNode?, _ sum: Int, _ temp: inout [Int], _ result: inout [[Int]]) {
        guard let node = node else { return }
        temp.append(node.val)
        switch (node.left, node.right) {
            case (.none, .none):
            if sum == node.val {
                result.append(temp)
            }
            default:
            pathSum(node.left, sum-node.val, &temp, &result)
            pathSum(node.right, sum-node.val, &temp, &result)
        }
        temp.removeLast()
    }
}
```