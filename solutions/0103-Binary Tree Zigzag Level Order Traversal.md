
### Binary Tree Zigzag Level Order Traversal

Given a binary tree, return the *zigzag level order* traversal of its nodes' values.</br> 
(ie, from left to right, then right to left for the next level and alternate between).

__For example:__
Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```
return its zigzag level order traversal as:
```
[
  [3],
  [20,9],
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
    func zigzagLevelOrder(_ root: TreeNode?) -> [[Int]] {
        guard let node = node else { return [] }
        var level = 0, queue : [TreeNode] = [node], result : [[Int]] = []
        while !queue.isEmpty {
            let size = queue.count
            var temp : [Int] = []
            for _ in 0..<size {
                let node = queue.removeFirst()
                if level%2 == 0 {
                    temp.append(node.val)
                } else {
                    temp.insert(node.val, at: 0)
                }
                if let left = node.left {
                    queue.append(left)
                }
                if let right = node.right {
                    queue.append(right)
                }
            }
            result.append(temp)
            level+=1
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
    func zigzagLevelOrder(_ root: TreeNode?) -> [[Int]] {
        var result : [[Int]] = []
        preOrder(root, 0, &result)
        return result
    }
    
    func preOrder(_ node: TreeNode?, _ level: Int, _ result: inout [[Int]]) {
        guard let node = node else { return }
        if level == result.count {
            result.append([Int]())
        }
        if level%2 == 0 {
            result[level].append(node.val)
        } else {
            result[level].insert(node.val, at: 0)
        }
        preOrder(node.left, level+1, &result)
        preOrder(node.right, level+1, &result)
    }
}
```