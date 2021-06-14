
### Binary Tree Level Order Traversal

Given a binary tree, return the *level order* traversal of its nodes' values. (ie, from left to right, level by level).

__For example:__
Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its level order traversal as:
```
[
  [3],
  [9,20],
  [15,7]
]
```

### Solution
__Iterative:__
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
    func levelOrder(_ root: TreeNode?) -> [[Int]] {
        guard let root = root else { return [] }
        var queue: [TreeNode] = [root], result: [[Int]] = []
        while !queue.isEmpty {
            let size = queue.count
            var temp: [Int] = []
            for _ in 0 ..< size {
                let node = queue.removeFirst()
                temp.append(node.val)
                node.left.map { queue.append($0) }
                node.right.map { queue.append($0) }
            }
            result.append(temp)
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
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 * }
 */
class Solution {
    func levelOrder(_ root: TreeNode?) -> [[Int]] {
        var result : [[Int]] = []
        preOrder(root, 0, &result)
        return result
    }

    func preOrder(_ node: TreeNode?, _ level: Int, _ result: inout [[Int]]) {
        guard let node = node else { return }
        if result.count == level {
            result.append([Int]())
        }
        result[level].append(node.val)
        preOrder(node.left, level+1, &result)
        preOrder(node.right, level+1, &result)
    }
}
```