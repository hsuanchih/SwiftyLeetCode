
### Sum of Left Leaves

Find the sum of all left leaves in a given binary tree.

__Example:__
```
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
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
    func sumOfLeftLeaves(_ root: TreeNode?) -> Int {
        return sum(root, false)
    }
    
    func sum(_ node: TreeNode?, _ isLeft: Bool) -> Int {
        guard let node = node else { return 0 }
        switch (node.left, node.right) {
            case (.none, .none):
            return isLeft ? node.val : 0
            default:
            return sum(node.left, true) + sum(node.right, false)
        }
    }
}
```
__O(n) Time, O(n/2+n/4) Space - Iterative:__
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
 
extension TreeNode : Hashable {
    public static func == (lhs: TreeNode, rhs: TreeNode) -> Bool {
        return lhs === rhs
    }
    public func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }
}

class Solution {
    func sumOfLeftLeaves(_ root: TreeNode?) -> Int {
        guard let root = root else { return 0 }
        var queue = [root], leftNodes : Set<TreeNode> = [], result = 0
        while !queue.isEmpty {
            let curr = queue.removeLast()
            switch (curr.left, curr.right) {
                case (.none, .none):
                if leftNodes.contains(curr) {
                    result += curr.val
                    leftNodes.remove(curr)
                }
                default:
                if let left = curr.left {
                    queue.append(left)
                    leftNodes.insert(left)
                }
                if let right = curr.right {
                    queue.append(right)
                }
            }
        }
        return result
    }
}
```