
### Cousins in Binary Tree

In a binary tree, the root node is at depth `0`, and children of each depth `k` node are at depth `k+1`.

Two nodes of a binary tree are cousins if they have the same depth, but have __different parents__.

We are given the `root` of a binary tree with unique values, and the values `x` and `y` of two different nodes in the tree.

Return `true` if and only if the nodes corresponding to the values `x` and `y` are cousins.

__Example 1:__

![Example 1](images/question_993-0.png)
```
Input: root = [1,2,3,4], x = 4, y = 3
Output: false
```
__Example 2:__

![Example 2](images/question_993-1.png)
```
Input: root = [1,2,3,null,4,null,5], x = 5, y = 4
Output: true
```
__Example 3:__

![Example 3](images/question_993-2.png)
```
Input: root = [1,2,3,null,4], x = 2, y = 3
Output: false
```

__Note:__
1. The number of nodes in the tree will be between `2` and `100`.
2. Each node has a unique integer value from `1` to `100`.

### Solution
__O(n) Recursive:__
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
    typealias NodeInfo = (parent: Int, depth: Int)
    
    func isCousins(_ root: TreeNode?, _ x: Int, _ y: Int) -> Bool {
        guard let root = root else { return false }
        if let result = isCousins(root, x, y, root.val, 0), result.depth == Int.min {
            return true
        }
        return false
    }
    
    func isCousins(_ node: TreeNode?, _ x: Int, _ y: Int, _ parent: Int, _ depth: Int) -> NodeInfo? {
        guard let node = node else { return nil }
        if node.val == x || node.val == y {
            return NodeInfo(parent, depth)
        }
        switch (isCousins(node.left, x, y, node.val, depth+1), isCousins(node.right, x, y, node.val, depth+1)) {
            case let (.some(left), .some(right)):
            return left.parent != right.parent && left.depth == right.depth ? NodeInfo(parent: Int.min, depth: Int.min) : nil
            case let (left, right):
            return left != nil ? left : right
        }
    }
}
```
