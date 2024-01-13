
### Unique Binary Search Trees

Given *n*, how many structurally unique __BST's__ (binary search trees) that store values 1 ... *n*?

__Example:__
```
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

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
    func numTrees(_ n: Int) -> Int {
        guard n > 1 else { return 1 }
        var trees: Int = 0
        for root in 1 ... n {
            trees += numTrees(root - 1) * numTrees(n - root)
        }
        return trees
    }
}
```
__Recursive O(n^2):__
```Swift
class Solution {
    func numTrees(_ n: Int) -> Int {
        var memo: [Int?] = Array(repeating: nil, count: n + 1)
        return numTrees(n: n, memo: &memo)
    }

    func numTrees(n: Int, memo: inout [Int?]) -> Int {
        guard n > 1 else { return 1 }
        if let trees = memo[n] {
            return trees
        }
        var trees: Int = 0
        for root in 1 ... n {
            trees += numTrees(n: root - 1, memo: &memo) * numTrees(n: n - root, memo: &memo) 
        }
        memo[n] = trees
        return trees
    }
}
```
__Iterative O(n^2):__
```Swift
class Solution {
    func numTrees(_ n: Int) -> Int {
        guard n > 1 else { return 1 }
        var numTrees: [Int] = Array(repeating: 1, count: 2)
        for nodes in 2 ... n {
            var trees: Int = 0
            for root in 1 ... nodes {
                trees += numTrees[root - 1] * numTrees[nodes - root]
            }
            numTrees.append(trees)
        }
        return numTrees.last ?? 1
    }
}
```