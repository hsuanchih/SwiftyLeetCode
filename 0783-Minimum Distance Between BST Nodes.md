
### Minimum Distance Between BST Nodes

Given a Binary Search Tree (BST) with the root node `root`,</br> 
return the minimum difference between the values of any two different nodes in the tree.

__Example:__
```
Input: root = [4,2,6,1,3,null,null]
Output: 1
Explanation:
Note that root is a TreeNode object, not an array.

The given tree [4,2,6,1,3,null,null] is represented by the following diagram:

          4
        /   \
      2      6
     / \    
    1   3  

while the minimum difference in this tree is 1, it occurs between node 1 and node 2, also between node 3 and node 2.
```

__Note:__
1. The size of the BST will be between `2` and `100`.
2. The BST is always valid, each node's value is an integer, and each node's value is different.

### Solution
__O(n):__
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
    func minDiffInBST(_ root: TreeNode?) -> Int {
        var result : [Int] = []
        inOrder(root, &result)
        var minDiff = Int.max
        for i in stride(from: 0, to: result.count-1, by: 1) {
            minDiff = min(minDiff, abs(result[i]-result[i+1]))
        }
        return minDiff
    }
    
    func inOrder(_ node: TreeNode?, _ result: inout [Int]) {
        guard let node = node else { return }
        inOrder(node.left, &result)
        result.append(node.val)
        inOrder(node.right, &result)
    }
}
```