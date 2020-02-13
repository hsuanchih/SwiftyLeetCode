
### Path Sum II

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

__Note:__ A leaf is a node with no children.

__Example:__

Given the below binary tree and `sum = 22`,
```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```
Return:
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

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