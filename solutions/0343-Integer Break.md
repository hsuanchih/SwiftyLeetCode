
### Integer Break

Given a positive integer *n*, break it into the sum of __at least__ two positive integers and maximize the product of those integers. Return the maximum product you can get.

__Example 1:__
```
Input: 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.
```
__Example 2:__
```
Input: 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
```

__Note:__ You may assume that *n* is not less than 2 and not larger than 58.

### Solution
__O(n^(log(n))) Recursive Top-Down - Brute-Force:__
```Swift
class Solution {
    func integerBreak(_ n: Int) -> Int {
        switch n {
            case 2: return 1
            case 3: return 2
            default:
            return maxProduct(n)
        }
    }
    
    func maxProduct(_ n: Int) -> Int {
        switch n {
            case 1...3:
            return n
            default:
            var result = 0
            for num in 2...n/2 {
                result = max(result, maxProduct(num)*maxProduct(n-num))
            }
            return result
        }
    }
}
```
__O(n^2) Recursive Top-Down - Memoization:__
```Swift
class Solution {
    func integerBreak(_ n: Int) -> Int {
        var memo : [Int] = Array(repeating: 0, count: n+1)
        switch n {
            case 2: return 1
            case 3: return 2
            default:
            return maxProduct(n, &memo)
        }
    }
    
    func maxProduct(_ n: Int, _ memo: inout [Int]) -> Int {
        switch n {
            case 1...3:
            return n
            default:
            if memo[n] > 0 {
                return memo[n]
            }
            var result = 0
            for num in 2...n/2 {
                result = max(result, num*maxProduct(n-num, &memo))
            }
            memo[n] = result
            return memo[n]
        }
    }
}
```
__O(n^2) Iterative Bottom-Up - Memoization:__
```Swift
class Solution {
    func integerBreak(_ n: Int) -> Int {
        var memo : [Int] = Array(0...n)
        switch n {
            case 2: return 1
            case 3: return 2
            default:
            for num in 4...n {
                var maxProduct = 0
                for k in 2...num/2 {
                    maxProduct = max(maxProduct, k*memo[num-k])
                }
                memo[num] = maxProduct
            }
        }
        return memo.last!
    }
}
```