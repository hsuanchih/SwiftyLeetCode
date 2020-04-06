
### Find Mode in Binary Search Tree

Given a binary search tree (BST) with duplicates, find all the mode(s) (the most frequently occurred element) in the given BST.

Assume a BST is defined as follows:
* The left subtree of a node contains only nodes with keys __less than or equal to__ the node's key.
* The right subtree of a node contains only nodes with keys __greater than or equal to__ the node's key.
* Both the left and right subtrees must also be binary search trees.

__For example:__
Given BST [1,null,2,2],
```
   1
    \
     2
    /
   2
```
return `[2]`.

__Note:__ If a tree has more than one mode, you can return them in any order.
__Follow up:__ Could you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).

### Solution
__O(n+n) Time, O(n) Space - In-Order HashMap Construction:__
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
    func findMode(_ root: TreeNode?) -> [Int] {
        var lookup : [Int: Int] = [:], maxFreq = 0
        inOrder(root, &lookup, &maxFreq)
        return Array(lookup.filter { $0.value == maxFreq }.keys)
    }
    
    func inOrder(_ node: TreeNode?, _ lookup: inout [Int: Int], _ maxFreq: inout Int) {
        guard let node = node else { return }
        inOrder(node.left, &lookup, &maxFreq)
        lookup[node.val, default: 0]+=1
        maxFreq = max(maxFreq, lookup[node.val]!)
        inOrder(node.right, &lookup, &maxFreq)
    }
}
```