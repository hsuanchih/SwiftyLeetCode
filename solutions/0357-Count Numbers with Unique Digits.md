
### Count Numbers with Unique Digits

Given a __non-negative__ integer `n`, count all numbers with unique digits, `x`, where `0 ≤ x < 10^n`.

__Example:__
```
Input: 2
Output: 91 
Explanation: The answer should be the total numbers in the range of 0 ≤ x < 100, 
             excluding 11,22,33,44,55,66,77,88,99
```

### Solution
__O((n^2)/2) Time, O(1) Space - Brute-Force Recursive:__
```Swift
class Solution {
    func countNumbersWithUniqueDigits(_ n: Int) -> Int {
        // Deducing the rules as follows:
        // n == 0: There is only 1 number with unique digits - 0
        // n == 1: Numbers 1...9 are unique - 9
        // n == 2: The possibilities are [1..9]*(10-1) - 9*(10-1)
        // n == 3: The possibilities are [1..9]*(10-1)*(10-2) - 9*(10-1)*(10-2)
        // Thus, for any n except 0:
        // Numbers with unique digits for 10^(n-1)..<10^(n) is
        // 9*(10-1)*(10-2)*..(10-(n-1))
        
        // Base Case:
        if n == 0 {
            return 1
        }
        
        // Compute # of numbers with unique digits for 10^(n-1)..<10^(n)
        var result = 9
        for i in 1..<n {
            result*=(10-i)
        }
        // Add the # of numbers with unique digits from number set with one fewer digit
        return result+countNumbersWithUniqueDigits(n-1)
    }
}
```
__O(n) Time, O(n) Space - Top-Down Recursive, Bottom-Up Memoization:__
```Swift
class Solution {
    func countNumbersWithUniqueDigits(_ n: Int) -> Int {

        // Same solution as brute-force, but this time with memoization
        var memo : [Int?] = Array(repeating: nil, count: n+1)
        return countNumbersWithUniqueDigits(n, &memo)
    }
    
    func countNumbersWithUniqueDigits(_ n: Int, _ memo: inout [Int?]) -> Int {
        if n == 0 {
            return 1
        }
        if let result = memo[n] {
            return result
        }
        var result = 9
        for i in 1..<n {
            result*=(10-i)
        }
        memo[n] = result+countNumbersWithUniqueDigits(n-1, &memo)
        return memo[n]!
    }
}
```
__O(n) Time, O(n) Space - Bottom-Up Iterative, Bottom-Up Memoization:__
```Swift
class Solution {
    func countNumbersWithUniqueDigits(_ n: Int) -> Int {
        var memo : [Int] = Array(repeating: 0, count: n+1)
        for exponent in 0...n {
            if exponent == 0 {
                memo[exponent] = 1
                continue
            }
            var result = 9
            for i in 1..<exponent {
                result*=(10-i)
            }
            memo[exponent] = result+memo[exponent-1]
        }
        return memo.last ?? 0
    }
}
```