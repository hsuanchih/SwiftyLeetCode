
### House Robber III

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

__Example 1:__
```
Input: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

Output: 7 
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```
__Example 2:__
```
Input: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
```

### Solution
__Brute-Force:__
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
    public func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }
}
extension TreeNode : Equatable {
    public static func ==(lhs: TreeNode, rhs: TreeNode) -> Bool {
        return ObjectIdentifier(lhs) == ObjectIdentifier(rhs)
    }
}
class Solution {
    func rob(_ root: TreeNode?) -> Int {
        return rob(root, true)
    }
    
    func rob(_ node: TreeNode?, _ canRob: Bool) -> Int {
        guard let node = node else { return 0 }
        if canRob {
            return max(
                node.val + rob(node.left, false) + rob(node.right, false),
                rob(node.left, true) + rob(node.right, true)
            )
        } else {
            return rob(node.left, true) + rob(node.right, true)
        }
    }
}
```
__O(2*n) Time - Memoization:__
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
    public func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }
}
extension TreeNode : Equatable {
    public static func ==(lhs: TreeNode, rhs: TreeNode) -> Bool {
        return ObjectIdentifier(lhs) == ObjectIdentifier(rhs)
    }
}
class Solution {
    func rob(_ root: TreeNode?) -> Int {
        var memo : [[TreeNode: Int]] = Array(repeating: [:], count: 2)
        return rob(root, true, &memo)
    }
    
    func rob(_ node: TreeNode?, _ canRob: Bool, _ memo: inout [[TreeNode: Int]]) -> Int {
        guard let node = node else { return 0 }
        if canRob, let result = memo[1][node] {
            return result
        }
        if !canRob, let result = memo[0][node] {
            return result
        }
        if canRob {
            memo[1][node] = max(
                rob(node.left, true, &memo) + rob(node.right, true, &memo),
                node.val + rob(node.left, false, &memo) + rob(node.right, false, &memo)
            )
        } else {
            memo[0][node] = rob(node.left, true, &memo) + rob(node.right, true, &memo)
        }
        return canRob ? memo[1][node]! : memo[0][node]!
    }
}
```