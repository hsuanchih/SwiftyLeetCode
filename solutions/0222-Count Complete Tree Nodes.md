
### Count Complete Tree Nodes

Given the `root` of a __complete binary tree__, return the number of the nodes in the tree.

According to Wikipedia, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `pow(2, h)` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

__Example 1:__

![question_222.jpg](../images/question_222.jpg)
```
Input: root = [1,2,3,4,5,6]
Output: 6
```
__Example 2:__
```
Input: root = []
Output: 0
```
__Example 3:__
```
Input: root = [1]
Output: 1
```

__Constraints:__
* The number of nodes in the tree is in the range `[0, 5 * pow(10, 4)]`.
* `0 <= Node.val <= 5 * pow(10, 4)`
* The tree is guaranteed to be __complete__.

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
    func countNodes(_ root: TreeNode?) -> Int {
        guard let node = root else { return 0 }
        return 1 + countNodes(node.left) + countNodes(node.right)
    }
}
```