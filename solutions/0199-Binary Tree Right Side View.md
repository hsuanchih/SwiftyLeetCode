
### Binary Tree Right Side View

Given a binary tree, imagine yourself standing on the *right* side of it, return the values of the nodes you can see ordered from top to bottom.

__Example:__
```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

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