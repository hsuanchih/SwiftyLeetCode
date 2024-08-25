
### Symmetric Tree

Given the `root` of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

__Example 1:__

![question_101-0.jpg](../images/question_101-0.jpg)
```
Input: root = [1,2,2,3,4,4,3]
Output: true
```
__Example 2:__

![question_101-1.jpg](../images/question_101-1.jpg)
```
Input: root = [1,2,2,null,3,null,3]
Output: false
```

__Constraints:__
* The number of nodes in the tree is in the range `[1, 1000]`.
* `-100 <= Node.val <= 100`

__Follow up:__ 
* Could you solve it both recursively and iteratively?

### Solution
__O(n) Time, O(1) Space - Recursive:__
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
    func isSymmetric(_ root: TreeNode?) -> Bool {
        return isMirror(root?.left, root?.right)
    }
    
    func isMirror(_ lhs: TreeNode?, _ rhs: TreeNode?) -> Bool {
        switch (lhs?.val, rhs?.val) {
            case (.none, .none):
            return true
            case let (left, right) where left == right:
            return isMirror(lhs?.left, rhs?.right) && isMirror(lhs?.right, rhs?.left)
            default:
            return false
        }
    }
}
```
__O(2n) Time, O(n/2) Space - Iterative:__
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
    func isSymmetric(_ root: TreeNode?) -> Bool {
        guard let root = root else { return true }
        var queue: [TreeNode] = [root]

        // Run level order traversal, stub a dummy node if the left or right child is nil
        while !queue.isEmpty {
            let size: Int = queue.count
            var temp: [Int] = []
            for _ in 0 ..< size {
                let node = queue.removeFirst()
                temp.append(node.val)

                // Stub left & right children only if the node itself is not a stub,
                // otherwise the traversal will never termintate
                if node.val != Int.max {
                    queue.append(node.left ?? TreeNode(Int.max))
                    queue.append(node.right ?? TreeNode(Int.max))
                }
            }

            // A tree is symmetric only if elements at every level is symmetric
            if temp != temp.reversed() {
                return false
            }
        }
        return true
    }
}
```
