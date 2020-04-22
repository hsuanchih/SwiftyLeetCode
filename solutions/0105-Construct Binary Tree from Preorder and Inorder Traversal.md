
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
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 * }
 */
class Solution {
    func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
        return build(preorder, 0, preorder.count-1, inorder, 0, inorder.count-1)
    }
    
    func build(_ preorder: [Int], _ pStart : Int, _ pEnd: Int, _ inorder: [Int], _ iStart: Int, _ iEnd: Int) -> TreeNode? {
        if pStart > pEnd {
            return nil
        }
        let currValue = preorder[pStart], currNode = TreeNode(currValue)
        let midIndex = inorder.firstIndex(of: currValue)!
        currNode.left = build(preorder, pStart+1, pStart+midIndex-iStart, inorder, iStart, midIndex-1)
        currNode.right = build(preorder, pStart+midIndex-iStart+1, pEnd, inorder, midIndex+1, iEnd)
        return currNode
    }
}
```