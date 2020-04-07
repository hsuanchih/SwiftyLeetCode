
### Average of Levels in Binary Tree

Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.

__Example 1:__
```
Input:
    3
   / \
  9  20
    /  \
   15   7
Output: [3, 14.5, 11]
Explanation:
The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].
```

__Note:__
1. The range of node's value is in the range of 32-bit signed integer.

### Solution
__O(n) Time, O(1) Space - Iterative:__
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
    func averageOfLevels(_ root: TreeNode?) -> [Double] {
        guard let root = root else { return [] }
        var queue : [TreeNode] = [root], result : [Double] = []
        while !queue.isEmpty {
            let size = queue.count
            var sum = 0
            for _ in 0..<size {
                let node = queue.removeFirst()
                if let left = node.left {
                    queue.append(left)
                }
                if let right = node.right {
                    queue.append(right)
                }
                sum+=node.val
            }
            result.append(Double(sum)/Double(size))
        }
        return result
    }
}
```
__O(n) Time, O(n) Space - Pre-Order Recursive:__
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
    func averageOfLevels(_ root: TreeNode?) -> [Double] {
        var result : [[Int]] = []
        preOrder(root, 0, &result)
        return result.map { level in
            Double(level.reduce(0) { $0+$1 })/Double(level.count)
        }
    }
    
    func preOrder(_ node: TreeNode?, _ level: Int, _ result: inout [[Int]]) {
        guard let node = node else { return }
        if level == result.count {
            result.append([Int]())
        }
        result[level].append(node.val)
        preOrder(node.left, level+1, &result)
        preOrder(node.right, level+1, &result)
    }
}
```