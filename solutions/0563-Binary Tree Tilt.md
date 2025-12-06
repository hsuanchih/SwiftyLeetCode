
### Binary Tree Tilt

Given a binary tree, return the tilt of the __whole tree__.

The tilt of a __tree node__ is defined as the __absolute difference__ between the sum of</br> 
all left subtree node values and the sum of all right subtree node values.</br> 
Null node has tilt 0.

The tilt of the __whole tree__ is defined as the sum of all nodes' tilt.

__Example:__
```
Input: 
         1
       /   \
      2     3
Output: 1
Explanation: 
Tilt of node 2 : 0
Tilt of node 3 : 0
Tilt of node 1 : |2-3| = 1
Tilt of binary tree : 0 + 0 + 1 = 1
```

__Note:__
1. The sum of node values in any subtree won't exceed the range of 32-bit integer.
2. All the tilt values won't exceed the range of 32-bit integer.

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
    func findTilt(_ root: TreeNode?) -> Int {
        var tilt = 0
        solve(root, &tilt)
        return tilt
    }

    func solve(_ node: TreeNode?, _ tilt: inout Int) -> Int {
        guard let node = node else { return 0 }
        let left = solve(node.left, &tilt), right = solve(node.right, &tilt)
        tilt += abs(left-right)
        return node.val+left+right
    }
}
```