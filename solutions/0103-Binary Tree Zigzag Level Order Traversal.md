
### Binary Tree Zigzag Level Order Traversal

Given the `root` of a binary tree, return the _zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

__Example 1:__

![question_103.jpg](../images/question_103.jpg)
```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
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
* `-100 <= Node.val <= 100`

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
    func zigzagLevelOrder(_ root: TreeNode?) -> [[Int]] {
        guard let root else { return [] }
        var queue: [TreeNode] = [root]
        var result: [[Int]] = []
        while !queue.isEmpty {
            let count: Int = queue.count
            let isOddLevel: Bool = result.count % 2 == 1
            var values: [Int] = []
            for _ in 0 ..< count {
                let node: TreeNode = queue.removeFirst()
                if isOddLevel {
                    values.insert(node.val, at: 0)
                } else {
                    values.append(node.val)
                }
                [node.left, node.right].forEach {
                    guard let next = $0 else { return }
                    queue.append(next)
                }
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
    func zigzagLevelOrder(_ root: TreeNode?) -> [[Int]] {
        var result: [[Int]] = []
        preorder(root, 0, &result)
        return result
    }
    
    func preorder(_ node: TreeNode?, _ level: Int, _ result: inout [[Int]]) {
        guard let node else { return }
        if level == result.count {
            result.append([])
        }
        if level % 2 == 0 {
            result[level].append(node.val)
        } else {
            result[level].insert(node.val, at: 0)
        }
        [node.left, node.right].forEach {
            preorder($0, level + 1, &result)
        }
    }
}
```