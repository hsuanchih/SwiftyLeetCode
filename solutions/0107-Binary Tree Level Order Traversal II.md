
### Binary Tree Level Order Traversal II

Given a binary tree, return the *bottom-up level order* traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

__For example:__
Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its bottom-up level order traversal as:
```
[
  [15,7],
  [9,20],
  [3]
]
```

### Solution
__O(n) Time, O(n/2) Space - Iterative:__
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
    func levelOrderBottom(_ root: TreeNode?) -> [[Int]] {
        guard let root = root else { return [] }
        var queue : [TreeNode] = [root], result : [[Int]] = []
        while !queue.isEmpty {
            let size = queue.count
            var temp : [Int] = []
            for _ in 0 ..< size {
                let node = queue.removeFirst()
                if let left = node.left {
                    queue.append(left)
                }
                if let right = node.right {
                    queue.append(right)
                }
                temp.append(node.val)
            }
            result.append(temp)
        }
        return result.reversed()
    }
}
```
__O(n) Time, O(1) Space - Recursive Pre-Order:__
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
    func levelOrderBottom(_ root: TreeNode?) -> [[Int]] {
        var result : [[Int]] = []
        preOrder(root, 0, &result)
        return result.reversed()
    }
    
    func preOrder(_ node: TreeNode?, _ level: Int, _ result: inout [[Int]]) {
        guard let node = node else { return }
        if level == result.count {
            result.append([])
        }
        result[level].append(node.val)
        preOrder(node.left, level+1, &result)
        preOrder(node.right, level+1, &result)
    }
}
```