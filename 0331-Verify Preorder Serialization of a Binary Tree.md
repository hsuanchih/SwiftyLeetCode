
### Longest Increasing Path in a Matrix

One way to serialize a binary tree is to use pre-order traversal. When we encounter a non-null node, we record the node's value.</br> 
If it is a null node, we record using a sentinel value such as `#`.
```
     _9_
    /   \
   3     2
  / \   / \
 4   1  #  6
/ \ / \   / \
# # # #   # #
```
For example, the above binary tree can be serialized to the string `"9,3,4,#,#,1,#,#,2,#,6,#,#"`, where `#` represents a null node.

Given a string of comma separated values, verify whether it is a correct preorder traversal serialization of a binary tree.</br> 
Find an algorithm without reconstructing the tree.

Each comma separated value in the string must be either an integer or a character `'#'` representing `null` pointer.

You may assume that the input format is always valid, for example it could never contain two consecutive commas such as "1,,3".

__Example 1:__
```
Input: "9,3,4,#,#,1,#,#,2,#,6,#,#"
Output: true
```
__Example 2:__
```
Input: "1,#"
Output: false
```
__Example 3:__
```
Input: "9,#,#,1"
Output: false
```

### Solution
__O(preorder) Time, O(1) Space:__
```Swift
class Solution {
    func isValidSerialization(_ preorder: String) -> Bool {
        let preorder = preorder.split(separator: ",")
        var edges = 1
        for string in preorder {
            edges-=1
            guard edges >= 0 else { return false }
            switch string {
                case "#":
                break
                default:
                edges+=2
            }
        }
        return edges == 0
    }
}
```