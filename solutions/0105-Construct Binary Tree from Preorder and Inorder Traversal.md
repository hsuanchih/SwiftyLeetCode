
### Construct Binary Tree from Preorder and Inorder Traversal

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return the binary tree.

__Example 1:__

![question_105.jpg](../images/question_105.jpg)
```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```
__Example 2:__
```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

__Constraints:__
* `1 <= preorder.length <= 3000`
* `inorder.length == preorder.length`
* `-3000 <= preorder[i], inorder[i] <= 3000`
* `preorder` and `inorder` consist of unique values.
* Each value of `inorder` also appears in `preorder`.
* `preorder` is guaranteed to be the preorder traversal of the tree.
* `inorder` is guaranteed to be the inorder traversal of the tree.

### Solution
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
    func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
        build(
            preorder,
            inorder,
            ClosedRange(uncheckedBounds: (0, preorder.count - 1)),
            ClosedRange(uncheckedBounds: (0, inorder.count - 1))
        )
    }

    func build(_ preorder: [Int], _ inorder: [Int], _ pRange: ClosedRange<Int>, _ iRange: ClosedRange<Int>) -> TreeNode? {
        if pRange.lowerBound <= pRange.upperBound {
            let value: Int = preorder[pRange.lowerBound]
            if let nodeIndex = inorder.firstIndex(where: { $0 == value }) {
                let numLeftNodes: Int = nodeIndex - iRange.lowerBound
                let node: TreeNode = TreeNode(value)
                node.left = build(
                    preorder,
                    inorder,
                    ClosedRange(uncheckedBounds: (pRange.lowerBound + 1, pRange.lowerBound + numLeftNodes)),
                    ClosedRange(uncheckedBounds: (iRange.lowerBound, nodeIndex - 1))
                )
                node.right = build(
                    preorder,
                    inorder,
                    ClosedRange(uncheckedBounds: (pRange.lowerBound + numLeftNodes + 1, pRange.upperBound)),
                    ClosedRange(uncheckedBounds: (nodeIndex + 1, iRange.upperBound))
                )
                return node
            } else {
                fatalError()
            }
        } else {
            return nil
        }
    }
}
```