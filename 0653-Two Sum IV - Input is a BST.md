
### Two Sum IV - Input is a BST

Given a Binary Search Tree and a target number, return true if there exist two elements in the BST</br> 
such that their sum is equal to the given target.

__Example 1:__
```
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
```
__Example 2:__
```
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 28

Output: False
```

### Solution
__O(n+log(n)):__
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
    func findTarget(_ root: TreeNode?, _ k: Int) -> Bool {
        var array : [Int] = []
        inOrder(root, &array)
        var start = 0, end = array.count-1
        while start < end {
            switch array[start] + array[end] {
                case k:
                return true
                case Int.min..<k:
                start+=1
                default:
                end-=1
            }
        }
        return false
    }
    
    func inOrder(_ node: TreeNode?, _ result: inout [Int]) {
        guard let node = node else { return }
        inOrder(node.left, &result)
        result.append(node.val)
        inOrder(node.right, &result)
    }
}
```