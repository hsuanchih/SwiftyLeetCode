
### Construct Binary Tree from Inorder and Postorder Traversal

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return the binary tree.

__Example 1:__

![question_106.jpg](../images/question_106.jpg)
```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```
__Example 2:__
```
Input: inorder = [-1], postorder = [-1]
Output: [-1]
```

__Constraints:__
* `1 <= inorder.length <= 3000`
* `postorder.length == inorder.length`
* `-3000 <= inorder[i], postorder[i] <= 3000`
* `inorder` and `postorder` consist of unique values.
* Each value of `postorder` also appears in `inorder`.
* `inorder` is guaranteed to be the inorder traversal of the tree.
* `postorder` is guaranteed to be the postorder traversal of the tree.

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
    func buildTree(_ inorder: [Int], _ postorder: [Int]) -> TreeNode? {
        guard !inorder.isEmpty, inorder.count == postorder.count else { fatalError() }
        return build(inorder, 0 ... inorder.count - 1, postorder, 0 ... postorder.count - 1)
    }

    func build(_ inorder: [Int], _ iRange: ClosedRange<Int>, _ postorder: [Int], _ pRange: ClosedRange<Int>) -> TreeNode? {
        if pRange.lowerBound <= pRange.upperBound {
            let val: Int = postorder[pRange.upperBound]
            if let mid: Int = inorder.firstIndex(of: val) {
                let node: TreeNode = TreeNode(val)
                let numsLeft: Int = mid - iRange.lowerBound
                let numsRight: Int = iRange.upperBound - mid
                node.left = build(
                    inorder,
                    ClosedRange(uncheckedBounds: (iRange.lowerBound, mid - 1)),
                    postorder,
                    ClosedRange(uncheckedBounds: (pRange.lowerBound, pRange.lowerBound + numsLeft - 1))
                )
                node.right = build(
                    inorder,
                    ClosedRange(uncheckedBounds: (mid + 1, iRange.upperBound)),
                    postorder,
                    ClosedRange(uncheckedBounds: (pRange.lowerBound + numsLeft, pRange.upperBound - 1))
                )
                return node
            }
        }
        return nil
    }
}
```
