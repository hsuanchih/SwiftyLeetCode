
### Unique Binary Search Trees II

Given an integer *n*, generate all structurally unique __BST's__ (binary search trees) that store values 1 ... *n*.

__Example:__
```
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

### Solution
__O(2^n):__
```Swift
class Solution {
    func generateTrees(_ n: Int) -> [TreeNode?] {
        if n < 1 {
            return []
        }
        return solve(1, n)
    }
    
    func solve(_ start: Int, _ end: Int) -> [TreeNode?] {
        if start > end {
            return [nil]
        }
        var result : [TreeNode?] = []
        for i in start ... end {
            let left = solve(start, i-1), right = solve(i+1, end)
            
            left.forEach { (leftSubtree) in
                right.forEach { (rightSubtree) in
                    let root = TreeNode(i)
                    root.left = leftSubtree
                    root.right = rightSubtree
                    result.append(root)
                }
            }
        }
        return result
    }
}
```