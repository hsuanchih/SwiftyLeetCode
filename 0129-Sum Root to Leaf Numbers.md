
### Sum Root to Leaf Numbers

Given a binary tree containing digits from `0-9` only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path `1->2->3` which represents the number `123`.

Find the total sum of all root-to-leaf numbers.

__Note:__ A leaf is a node with no children.

__Example:__
```
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```
__Example 2:__
```
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

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
    func sumNumbers(_ root: TreeNode?) -> Int {
        var result = 0
        sumNumbers(root, 0, &result)
        return result
    }
    
    func sumNumbers(_ node: TreeNode?, _ sum: Int, _ result: inout Int) {
        guard let node = node else { return }
        let sum = sum*10+node.val
        switch (node.left, node.right) {
            case (.none, .none):
            result += sum
            default:
            sumNumbers(node.left, sum, &result)
            sumNumbers(node.right, sum, &result)
        }
    }
}
```