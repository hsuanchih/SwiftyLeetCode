
### Same Tree

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

__Example 1:__
```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true
```
__Example 2:__
```
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false
```
__Example 3:__
```
Input:     1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

Output: false
```

### Solution
__O(min(left,right)) Time, O(1) Space - Recursive:__
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
    func isSameTree(_ left: TreeNode?, _ right: TreeNode?) -> Bool {
        switch (left, right) {
            case let (.some(left), .some(right)):
            return left.val == right.val && isSameTree(left.left, right.left) && isSameTree(left.right, right.right)
            case (.none, .none):
            return true
            default:
            return false
        }
    }
}
```
__O(min(left,right)) Time, O(min(left+right)/2) Space - Iterative:__
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
    func isSameTree(_ p: TreeNode?, _ q: TreeNode?) -> Bool {
        
        switch isEqual(p, q) {
            case true:
            break
            case false:
            return false
            default:
            return true
        }
        
        var qP = [p!], qQ = [q!]
        while !qP.isEmpty && !qQ.isEmpty {
            let currP = qP.removeFirst(), currQ = qQ.removeFirst()
            
            switch isEqual(currP.left, currQ.left) {
                case false:
                return false
                case true:
                qP.append(currP.left!)
                qQ.append(currQ.left!)
                default:
                break
            }
            
            switch isEqual(currP.right, currQ.right) {
                case false:
                return false
                case true:
                qP.append(currP.right!)
                qQ.append(currQ.right!)
                default:
                break
            }
        }
        return true
    }
    
    func isEqual(_ lhs: TreeNode?, _ rhs: TreeNode?) -> Bool? {
        switch (lhs?.val, rhs?.val) {
            case (.none, .none):
            return nil
            case let (.some(valP), .some(valQ)) where valP == valQ:
            return true
            default:
            return false
        }
    }
}
```