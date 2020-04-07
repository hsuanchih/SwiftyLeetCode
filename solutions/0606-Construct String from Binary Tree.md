
### Construct String from Binary Tree

You need to construct a string consists of parenthesis and integers</br> 
from a binary tree with the preorder traversing way.

The null node needs to be represented by empty parenthesis pair "()".</br> 
And you need to omit all the empty parenthesis pairs that don't affect the one-to-one mapping</br> 
relationship between the string and the original binary tree.

__Example 1:__
```
Input: Binary tree: [1,2,3,4]
       1
     /   \
    2     3
   /    
  4     

Output: "1(2(4))(3)"

Explanation: Originallay it needs to be "1(2(4)())(3()())", 
but you need to omit all the unnecessary empty parenthesis pairs. 
And it will be "1(2(4))(3)".
```
__Example 2:__
```
Input: Binary tree: [1,2,3,null,4]
       1
     /   \
    2     3
     \  
      4 

Output: "1(2()(4))(3)"

Explanation: Almost the same as the first example, 
except we can't omit the first parenthesis pair to break the one-to-one mapping relationship between the input and the output.
```

### Solution
__O(n) Time - Recursive:__
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
    func tree2str(_ t: TreeNode?) -> String {
        var result = ""
        preOrder(t, &result)
        return result
    }
    
    func preOrder(_ node: TreeNode?, _ result: inout String) {
        guard let node = node else { return }
        switch (node.left, node.right) {
        case (.none, .none):
            result.append("\(node.val)")
        default:
            result.append("\(node.val)(")
            preOrder(node.left, &result)
            if let _ = node.right {
                result.append(")(")
                preOrder(node.right, &result)
                result.append(")")
            } else {
                result.append(")")
            }
        }
    }
}
```