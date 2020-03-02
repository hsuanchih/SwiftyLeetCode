
### Minimum Absolute Difference in BST

Given a binary search tree with non-negative values, find the minimum absolute difference between values of any two nodes.

__Example:__
```
Input:

   1
    \
     3
    /
   2

Output:
1

Explanation:
The minimum absolute difference is 1, which is the difference between 2 and 1 (or between 2 and 3).
```

### Solution
__O(n+n) Time, O(n) Space:__
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
    func getMinimumDifference(_ root: TreeNode?) -> Int {
        var input : [Int] = []
        inOrder(root, &input)
        var result = Int.max
        for i in stride(from: 0, to: input.count-1, by: 1) {
            result = min(result, abs(input[i]-input[i+1]))
        }
        return result
    }
    
    func inOrder(_ node: TreeNode?, _ result: inout [Int]) {
        guard let node = node else { return }
        inOrder(node.left, &result)
        result.append(node.val)
        inOrder(node.right, &result)
    }
}
```