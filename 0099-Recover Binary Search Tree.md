
### Recover Binary Search Tree

Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.

__Example 1:__
```
Input: [1,3,null,null,2]

   1
  /
 3
  \
   2

Output: [3,1,null,null,2]

   3
  /
 1
  \
   2
```
__Example 2:__
```
Input: [3,1,4,null,null,2]

  3
 / \
1   4
   /
  2

Output: [2,1,4,null,null,3]

  2
 / \
1   4
   /
  3
```
__Follow up:__
* A solution using O(n) space is pretty straight forward.
* Could you devise a constant space solution?

### Solution
__O(n) Time, O(n) Space - In-Order Traversal:__
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
    func recoverTree(_ root: TreeNode?) {
        var result : [TreeNode] = []
        inOrder(root, &result)

        // Iterate through the result of in-order traversal & identify the 2 nodes
        // that are out of place
        var first = -1, second = -1
        for i in 0..<result.count-1 {

            // If we run into a pair of values where the 1st value is greater than 2nd value
            // then the 1st value is one of the nodes we want to remedy
            // If by chance we run into another pair of values where the 1st value is greater than
            // the 2nd value, then the latter of the 2 is the one we want to swap with the value
            // we've identified earlier
            if result[i].val > result[i+1].val {
                if first == -1 {
                    first = i
                } else {
                    second = i+1
                }
            }
        }

        // If there is no second value to swap with the first value, then we know
        // that the nodes that are out of place are adjacent to each other
        // Otherwise swap first & second
        switch (first, second) {
            case (_, -1):
            swap(result[first], result[first+1])
            default:
            swap(result[first], result[second])
        }
    }
    
    // In-Order Traversal
    func inOrder(_ node: TreeNode?, _ result: inout [TreeNode]) {
        guard let node = node else { return }
        inOrder(node.left, &result)
        result.append(node)
        inOrder(node.right, &result)
    }
    
    // Helper method to swap the values of two nodes
    func swap(_ lhs: TreeNode, _ rhs: TreeNode) {
        let temp = lhs.val
        lhs.val = rhs.val
        rhs.val = temp
    }
}
```