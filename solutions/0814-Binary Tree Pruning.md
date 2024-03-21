
### Binary Tree Pruning

Given the `root` of a binary tree, return the same tree where every subtree (of the given tree) not containing a `1` has been removed.

A subtree of a node `node` is `node` plus every node that is a descendant of `node`.

__Example 1:__
![images/question_814-0.png](../images/question_814-0.png)
```
Input: root = [1,null,0,0,1]
Output: [1,null,0,null,1]
Explanation: 
Only the red nodes satisfy the property "every subtree not containing a 1".
The diagram on the right represents the answer.
```
__Example 2:__
![images/question_814-1.png](../images/question_814-1.png)
```
Input: root = [1,0,1,0,0,0,1]
Output: [1,null,1,null,1]
```
__Example 3:__
![images/question_814-2.png](../images/question_814-2.png)
```
Input: root = [1,1,0,1,1,0,1,0]
Output: [1,1,0,1,1,null,1]
```

__Constraints:__
* The number of nodes in the tree is in the range `[1, 200]`.
* Node.val is either `0` or `1`.

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
    func pruneTree(_ root: TreeNode?) -> TreeNode? {
        var root: TreeNode? = root
        if canPrune(root) {
            root = nil
        }
        return root
    }

    func canPrune(_ node: TreeNode?) -> Bool {
        guard let node else { return true }
        if canPrune(node.left) {
            node.left = nil
        }
        if canPrune(node.right) {
            node.right = nil
        }
        return node.left == nil && node.right == nil && node.val == 0
    }
}
```
