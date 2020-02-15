
### Binary Tree Right Side View

Given a binary tree, imagine yourself standing on the *right* side of it, return the values of the nodes you can see ordered from top to bottom.

__Example:__
```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

### Solution
__Iterative O(n):__
```Swift
class Solution {
    func rightSideView(_ root: TreeNode?) -> [Int] {
        guard let root = root else { return [] }
        var queue : [TreeNode] = [root], result : [Int] = []
        while !queue.isEmpty {
            let size = queue.count
            for i in 0 ..< size {
                let node = queue.removeFirst()
                if let left = node.left {
                    queue.append(left)
                }
                if let right = node.right {
                    queue.append(right)
                }
                if i == size-1 {
                    result.append(node.val)
                }
            }
        }
        return result
    }
}
```