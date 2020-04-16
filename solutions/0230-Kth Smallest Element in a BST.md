
### Kth Smallest Element in a BST

Given a binary search tree, write a function `kthSmallest` to find the __k__th smallest element in it.

__Note:__
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

__Example 1:__
```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```
__Example 2:__
```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

__Follow up:__
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently?</br> 
How would you optimize the kthSmallest routine?

### Solution
__O(n) Time, O(n) Space:__
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
    func kthSmallest(_ root: TreeNode?, _ k: Int) -> Int {
        var result : [Int] = []
        inOrder(root, &result)
        return result[k-1]
    }
    
    func inOrder(_ node: TreeNode?, _ result: inout [Int]) {
        guard let node = node else { return }
        inOrder(node.left, &result)
        result.append(node.val)
        inOrder(node.right, &result)
    }
}
```
__O(k) Time, O(1) Space:__
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
    func kthSmallest(_ root: TreeNode?, _ k: Int) -> Int {
        var k = k
        return inOrder(root, &k)!
    }
    
    func inOrder(_ node: TreeNode?, _ k: inout Int) -> Int? {
        guard let node = node else { return nil }
        if let num = inOrder(node.left, &k) { 
            return num
        }
        k-=1
        if k == 0 {
            return node.val
        }
        return inOrder(node.right, &k)
    }
}
```