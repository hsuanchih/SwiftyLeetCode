
### Binary Tree Level Order Traversal

Given the `root` of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

__Example 1:__

![question_102.jpg](../images/question_102.jpg)
```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```
__Example 2:__
```
Input: root = [1]
Output: [[1]]
```
__Example 3:__
```
Input: root = []
Output: []
```

__Constraints:__
* The number of nodes in the tree is in the range `[0, 2000]`.
* `-1000 <= Node.val <= 1000`

### Solution
__Iterative:__
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
    func levelOrder(_ root: TreeNode?) -> [[Int]] {
        guard let root else { return [] }
        var queue: [TreeNode] = [root]
        var result: [[Int]] = []
        while !queue.isEmpty {
            let count: Int = queue.count
            var values: [Int] = []
            for _ in 0 ..< count {
                let node: TreeNode = queue.removeFirst()
                [node.left, node.right].forEach {
                    guard let next = $0 else { return }
                    queue.append(next)
                }
                values.append(node.val)
            }
            result.append(values)
        }
        return result
    }
}
```
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
    func levelOrder(_ root: TreeNode?) -> [[Int]] {
        var result: [[Int]] = []
        traverse(root, 0, &result)
        return result
    }

    func traverse(_ node: TreeNode?, _ level: Int, _ result: inout [[Int]]) {
        guard let node else { return }
        if level == result.count {
            result.append([node.val])
        } else {
            result[level].append(node.val)
        }
        traverse(node.left, level + 1, &result)
        traverse(node.right, level + 1, &result)
    }
}
```