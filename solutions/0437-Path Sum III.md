
### Path Sum III

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf,</br> 
but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

__Example:__
```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

### Solution
__O(n^2):__
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
    
    func pathSum(_ root: TreeNode?, _ sum: Int) -> Int {
        var path : [Int] = [], result : Int = 0
        solve(root, sum, &path, &result)
        return result
    }
    
    func solve(_ node: TreeNode?, _ sum: Int, _ path: inout [Int], _ result: inout Int) {
        guard let node = node else { return }
        path.append(node.val)
        var total = sum
        for i in stride(from: path.count-1, through: 0, by: -1) {
            total-=path[i]
            if total == 0 {
                result+=1
            }
        }
        solve(node.left, sum, &path, &result)
        solve(node.right, sum, &path, &result)
        path.removeLast()
    }
}
```