
### Binary Tree Right Side View

Given the `root` of a binary tree, imagine yourself standing on the __right side__ of it, return the values of the nodes you can see ordered from top to bottom.

__Example 1:__

![question_199.jpg](../images/question_199.jpg)
```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
```
__Example 2:__
```
Input: root = [1,null,3]
Output: [1,3]
```
__Example 3:__
```
Input: root = []
Output: []
```

__Constraints:__
* The number of nodes in the tree is in the range `[0, 100]`.
* `-100 <= Node.val <= 100`

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
    func rightSideView(_ root: TreeNode?) -> [Int] {
        var result: [Int] = []
        traverse(root, 0, &result)
        return result
    }

    func traverse(_ node: TreeNode?, _ level: Int, _ result: inout [Int]) {
        guard let node else { return }
        if level == result.count {
            result.append(node.val)
        }
        traverse(node.right, level + 1, &result)
        traverse(node.left, level + 1, &result)
    }
}
```
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
    func rightSideView(_ root: TreeNode?) -> [Int] {
        guard let root else { return [] }
        var queue: [TreeNode] = [root]
        var result: [Int] = []
        while !queue.isEmpty {
            let count: Int = queue.count
            for i in 0 ..< count {
                let node: TreeNode = queue.removeFirst()
                [node.left, node.right].forEach {
                    guard let next = $0 else { return }
                    queue.append(next)
                }
                if i == count - 1 {
                    result.append(node.val)
                }
            }
        }
        return result
    }
}
```