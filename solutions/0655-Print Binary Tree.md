
### Print Binary Tree

Print a binary tree in an m\*n 2D string array following these rules:

1. The row number `m` should be equal to the height of the given binary tree.
2. The column number `n` should always be an odd number.
3. The root node's value (in string format) should be put in the exactly middle of the first row it can be put. The column and the row where the root node belongs will separate the rest space into two parts (__left-bottom part and right-bottom part__). You should print the left subtree in the left-bottom part and print the right subtree in the right-bottom part. The left-bottom part and the right-bottom part should have the same size. Even if one subtree is none while the other is not, you don't need to print anything for the none subtree but still need to leave the space as large as that for the other subtree. However, if two subtrees are none, then you don't need to leave space for both of them.
4. Each unused space should contain an empty string `""`.
5. Print the subtrees following the same rules.

__Example 1:__
```
Input:
     1
    /
   2
Output:
[["", "1", ""],
 ["2", "", ""]]
```
__Example 2:__
```
Input:
     1
    / \
   2   3
    \
     4
Output:
[["", "", "", "1", "", "", ""],
 ["", "2", "", "", "", "3", ""],
 ["", "", "4", "", "", "", ""]]
```
__Example 3:__
```
Input:
      1
     / \
    2   5
   / 
  3 
 / 
4 
Output:

[["",  "",  "", "",  "", "", "", "1", "",  "",  "",  "",  "", "", ""]
 ["",  "",  "", "2", "", "", "", "",  "",  "",  "",  "5", "", "", ""]
 ["",  "3", "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]
 ["4", "",  "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]]
```

__Note:__ The height of binary tree is in the range of `[1, 10]`.

### Solution
__O(nodes) Time, O(width * height) Space:__
```Swift
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init() { self.val = 0; self.left = nil; self.right = nil; }
 *     public init(_ val: Int) { self.val = val; self.left = nil; self.right = nil; }
 *     public init(_ val: Int, _ left: TreeNode?, _ right: TreeNode?) {
 *         self.val = val
 *         self.left = left
 *         self.right = right
 *     }
 * }
 */
class Solution {
    func printTree(_ root: TreeNode?) -> [[String]] {
        let height: Int = findHeight(root)
        let width: Int = (1 << height) - 1
        var result: [[String]] = Array(repeating: Array(repeating: "", count: width), count: height)
        printNode(root, 0, ClosedRange(uncheckedBounds: (0, width - 1)), &result)
        return result
    }

    func printNode(_ node: TreeNode?, _ row: Int, _ range: ClosedRange<Int>, _ result: inout [[String]]) {
        guard let node else { return }
        let mid: Int = range.lowerBound + (range.upperBound - range.lowerBound) / 2
        result[row][mid] = "\(node.val)"
        printNode(node.left, row + 1, ClosedRange(uncheckedBounds: (range.lowerBound, mid - 1)), &result)
        printNode(node.right, row + 1, ClosedRange(uncheckedBounds: (mid + 1, range.upperBound)), &result)
    }

    func findHeight(_ node: TreeNode?) -> Int {
        guard let node else { return 0 }
        return max(findHeight(node.left), findHeight(node.right)) + 1
    }
}
```