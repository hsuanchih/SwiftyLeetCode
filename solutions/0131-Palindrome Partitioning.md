
### Palindrome Partitioning

Given a string s, partition *s* such that every substring of the partition is a palindrome.
Return all possible palindrome partitioning of *s*.

__Example:__
```
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```

### Solution
__O(2^s) Time:__
```Swift
class Solution {
    func partition(_ s: String) -> [[String]] {
        var ranges : [Range<Int>] = [], result : [[String]] = []
        partition(Array(s), &ranges, &result)
        return result
    }
    
    func partition(_ s: [Character], _ ranges : inout [Range<Int>], _ result: inout [[String]]) {
        let index = ranges.last?.upperBound ?? 0
        switch index {
            case s.count:
            result.append(ranges.map { String(s[$0]) })
            default:
            for i in index..<s.count {
                if isPalindrome(s, index, i) {
                    ranges.append(index..<i+1)
                    partition(s, &ranges, &result)
                    ranges.removeLast()
                }
            }
        }
    }
    
    func isPalindrome(_ s: [Character], _ start: Int, _ end: Int) -> Bool {
        if start == end {
            return true
        }
        var left = start, right = end
        while left < right {
            if s[left] != s[right] {
                return false
            }
            left+=1
            right-=1
        }
        return true
    }
}
```