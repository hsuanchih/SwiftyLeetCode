
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
__Recursive O(n):__
```Swift
class Solution {
    func numTrees(_ n: Int) -> Int {
        var memo : [Int?] = Array(repeating: nil, count: n+1)
        return numTrees(n, &memo)
    }
    
    func numTrees(_ n: Int, _ memo: inout [Int?]) -> Int {
        if n <= 1 {
            return 1
        }
        if let result = memo[n] {
            return result
        }
        var count = 0
        for root in stride(from: 1, through: n, by: 1) {
            count+=numTrees(root-1, &memo)*numTrees(n-root, &memo)
        }
        memo[n] = count
        return count
    }
}
```
__Iterative O(n):__
```Swift
class Solution {
    func numTrees(_ n: Int) -> Int {
        if n <= 1 {
            return 1
        }
        var memo : [Int] = Array(repeating: 1, count: n+1)
        for size in 2...n {
            var count = 0
            for root in 1...size {
                if root == 1 || root == size {
                    count += memo[size-1]
                } else {
                    count += (memo[root-1] * memo[size-root])
                }
            }
            memo[size] = count
        }
        return memo[n]
    }
}
```