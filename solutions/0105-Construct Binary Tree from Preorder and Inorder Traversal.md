
### Construct Binary Tree from Preorder and Inorder Traversal

Given preorder and inorder traversal of a tree, construct the binary tree.

__Note:__
You may assume that duplicates do not exist in the tree.

__For example__, given
```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```
Return the following binary tree:
```
    3
   / \
  9  20
    /  \
   15   7
```

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
        guard !preorder.isEmpty, preorder.count == inorder.count else { fatalError() }
        return build(preorder, 0 ... preorder.count - 1, inorder, 0 ... inorder.count - 1)
    }

    func build(_ preorder: [Int], _ pRange: ClosedRange<Int>, _ inorder: [Int], _ iRange: ClosedRange<Int>) -> TreeNode? {
        if pRange.lowerBound <= pRange.upperBound {
            let val: Int = preorder[pRange.lowerBound]
            if let mid: Int = inorder.firstIndex(of: val) {
                let node: TreeNode = TreeNode(val)
                let numsLeft: Int = mid - iRange.lowerBound
                let numsRight: Int = iRange.upperBound - mid
                node.left = build(
                    preorder, 
                    ClosedRange(uncheckedBounds: (pRange.lowerBound + 1, pRange.lowerBound + numsLeft)), 
                    inorder, 
                    ClosedRange(uncheckedBounds: (iRange.lowerBound, mid - 1))
                )
                node.right = build(
                    preorder, 
                    ClosedRange(uncheckedBounds: (pRange.lowerBound + numsLeft + 1, pRange.upperBound)), 
                    inorder, 
                    ClosedRange(uncheckedBounds: (mid + 1, iRange.upperBound))
                )
                return node
            }
        }
        return nil
    }
}
```