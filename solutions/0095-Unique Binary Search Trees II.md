
### Unique Binary Search Trees II

Given an integer `n`, return *all the structurally unique __BST's__ (binary search trees), which has exactly `n` nodes of unique values from `1` to `n`.* Return the answer in __any order__.

__Example 1:__

![question_95.jpg](../images/question_95.jpg)
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
```

__Example 2:__
```
Input: n = 1
Output: [[1]]
```

### Solution
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

__Using Closed Range Representation:__
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
    func generateTrees(_ n: Int) -> [TreeNode?] {
        generateTrees(ClosedRange(uncheckedBounds: (1, n)))
    }

    func generateTrees(_ range: ClosedRange<Int>) -> [TreeNode?] {
        switch range.upperBound - range.lowerBound {
        case Int.min ..< 0:
            return [nil]
        case 0:
            return [TreeNode(range.lowerBound)]
        case 1 ... Int.max:
            var trees: [TreeNode] = []
            for root in range {
                for leftSubtree in generateTrees(ClosedRange(uncheckedBounds: (range.lowerBound, root - 1))) {
                    for rightSubtree in generateTrees(ClosedRange(uncheckedBounds: (root + 1, range.upperBound))) {
                        let node = TreeNode(root)
                        node.left = leftSubtree
                        node.right = rightSubtree
                        trees.append(node)
                    }
                }
            }
            return trees
        default:
            fatalError()
        }
    }
}
```