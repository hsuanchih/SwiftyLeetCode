
### Find Bottom Left Tree Value

Given the `root` of a binary tree, return the leftmost value in the last row of the tree.
 
__Example 1:__

![images/question_513-0.jpg](../images/question_513-0.jpg)
```
Input: root = [2,1,3]
Output: 1
```
__Example 2:__

![images/question_513-1.jpg](../images/question_513-1.jpg)
```
Input: root = [1,2,3,4,null,5,6,null,null,7]
Output: 7
```

__Constraints:__
* The number of nodes in the tree is in the range `[1, pow(10, 4)]`.
* `-pow(2, 31) <= Node.val <= pow(2, 31) - 1`

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
    func findBottomLeftValue(_ root: TreeNode?) -> Int {
        var result: (level: Int, value: Int)?
        traverse(root, 0, &result)
        return result?.value ?? -1
    }

    func traverse(_ node: TreeNode?, _ level: Int, _ result: inout (level: Int, value: Int)?) {
        guard let node else { return }
        if level > (result?.level ?? -1) {
            result = (level, node.val)
        }
        traverse(node.left, level + 1, &result)
        traverse(node.right, level + 1, &result)
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
    func findBottomLeftValue(_ root: TreeNode?) -> Int {
        guard let root else { return -1 }
        var queue: [TreeNode] = [root]
        var result: Int = root.val
        while !queue.isEmpty {
            let count: Int = queue.count
            for i in 0 ..< count {
                let node: TreeNode = queue.removeFirst()
                [node.left, node.right].forEach {
                    guard let next = $0 else { return }
                    queue.append(next)
                }
                if i == 0 {
                    result = node.val
                }
            }
        }
        return result
    }
}
```
