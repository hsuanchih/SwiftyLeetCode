
### Second Minimum Node In a Binary Tree

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly `two` or `zero` sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property `root.val = min(root.left.val, root.right.val)` always holds.

Given such a binary tree, you need to output the __second minimum__ value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output `-1` instead.

__Example 1:__
```
Input: 
    2
   / \
  2   5
     / \
    5   7

Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.
```
__Example 2:__
```
Input: 
    2
   / \
  2   2

Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.
```

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
    func findSecondMinimumValue(_ root: TreeNode?) -> Int {
        guard let node = root else { return Int.max }
        let result = secondMin(node, node.val)
        return result == Int.max ? -1 : result
    }
    
    func secondMin(_ node: TreeNode?, _ min: Int) -> Int {
        guard let node = node else { return Int.max }
        switch node.val {
            case min:
            switch (secondMin(node.left, min), secondMin(node.right, min)) {
                case (min, min):
                return Int.max
                case (min, let val):
                return val
                case (let val, min):
                return val
                case let (lhs, rhs):
                return Swift.min(lhs, rhs)
            }
            default:
            return node.val
        }
    }
}
```
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
    func findSecondMinimumValue(_ root: TreeNode?) -> Int {
        guard let root = root else { return -1 }
        var queue = [root], min = root.val, result = Int.max
        while !queue.isEmpty {
            let curr = queue.removeFirst()
            if curr.val != min {
                result = Swift.min(result, curr.val)
            }
            if let left = curr.left {
                queue.append(left)
            }
            if let right = curr.right {
                queue.append(right)
            }
        }
        return result == Int.max ? -1 : result
    }
}
```