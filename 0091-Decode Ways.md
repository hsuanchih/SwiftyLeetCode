
### Decode Ways

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:
```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```
Given a __non-empty__ string containing only digits, determine the total number of ways to decode it.

__Example 1:__
```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```
__Example 2:__
```
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

### Solution
__O(2^n):__
```Swift
class Solution {
    func numDecodings(_ s: String) -> Int {
        return decodeWays(Array(s), 0)
    }
    
    func decodeWays(_ s: [Character], _ index: Int) -> Int {
        switch index {
        case s.count+1...Int.max:
            return 0
        case s.count:
            return 1
        default:
            break
        }
        if s[index] == "0" {
            return 0
        }
        var count = 0
        for i in stride(from: index, through: index+1, by: 1) where i < s.count {
            if s[i] == "0" && ( i == 0 || s[i-1] == "0") {
                return 0
            }
            let num = Int(String(s[index...i]))!
            if num >= 1 && num <= 26 {
                count+=decodeWays(s, i+1)
            }
        }
        return count
    }
}
```
__Recursive O(n):__
```Swift
class Solution {
    func numDecodings(_ s: String) -> Int {
        if s.isEmpty { return 0 }
        var memo : [Int?] = Array(repeating: nil, count: s.count)
        return decodeWays(Array(s), 0, &memo)
    }
    
    func decodeWays(_ s: [Character], _ index: Int, _ memo: inout [Int?]) -> Int {
        switch index {
        case s.count+1...Int.max:
            return 0
        case s.count:
            return 1
        default:
            break
        }
        if let result = memo[index] {
            return result
        }
        if s[index] == "0" {
            memo[index] = 0
            return 0
        }
        var count = 0
        for i in stride(from: index, through: index+1, by: 1) where i < s.count {
            let num = Int(String(s[index...i]))!
            if num >= 1 && num <= 26 {
                count+=decodeWays(s, i+1, &memo)
            }
        }
        memo[index] = count
        return count
    }
}
```
__Iterative O(n):__
```Swift
class Solution {
    func numDecodings(_ s: String) -> Int {
        let input = Array(s)
        var memo : [Int] = Array(repeating: 0, count: input.count)
        for i in stride(from: 0, to: input.count, by: 1) {
            if i == 0 {
                switch input[i] {
                    case "0":
                    return 0
                    default:
                    memo[i] = 1
                }
                continue
            }
            switch (input[i-1], input[i]) {
                case ("0", "0"):
                return 0
                case ("0", _):
                memo[i] = memo[i-1]
                case (_, "0"):
                if let num = Int(String(input[i-1...i])), num < 27 {
                    memo[i] = memo[i-1]
                } else {
                    memo[i] = 0
                }
                default:
                if let num = Int(String(input[i-1...i])), num < 27 {
                    memo[i] = memo[i-1] + 1
                } else {
                    memo[i] = memo[i-1]
                }
            }
        }
        return memo[memo.count-1]
    }
}
```