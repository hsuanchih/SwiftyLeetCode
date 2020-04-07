
### Range Sum of BST

Given the `root` node of a binary search tree, return the sum of values of all nodes with value between `L` and `R` (inclusive).

The binary search tree is guaranteed to have unique values.

__Example 1:__
```
Input: root = [10,5,15,3,7,null,18], L = 7, R = 15
Output: 32
```
__Example 2:__
```
Input: root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10
Output: 23
```

__Note:__
1. The number of nodes in the tree is at most `10000`.
2. The final answer is guaranteed to be less than `2^31`.

### Solution
__O(log\[base 2\](n)) Time - Recursive:__
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
    func rangeSumBST(_ root: TreeNode?, _ L: Int, _ R: Int) -> Int {
        guard let node = root else { return 0 }
        switch node.val {
            case Int.min..<L:
            return rangeSumBST(node.right, L, R)
            case R+1...Int.max:
            return rangeSumBST(node.left, L, R)
            default:
            return node.val + rangeSumBST(node.left, L, R) + rangeSumBST(node.right, L, R)
        }
    }
}
```