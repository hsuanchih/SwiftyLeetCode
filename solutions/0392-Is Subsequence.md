
### Is Subsequence

Given two strings `s` and `t`, return `true` if `s` is a subsequence of `t`, or `false` otherwise.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

__Example 1:__
```
Input: s = "abc", t = "ahbgdc"
Output: true
```
__Example 2:__
```
Input: s = "axc", t = "ahbgdc"
Output: false
```

__Constraints:__
* `0 <= s.length <= 100`
* `0 <= t.length <= pow(10, 4)`
* `s` and `t` consist only of lowercase English letters.

__Follow up:__ 
* Suppose there are lots of incoming `s`, say `s1, s2, ..., sk` where `k >= pow(10, 9)`, and you want to check one by one to see if `t` has its subsequence. In this scenario, how would you change your code?

### Solution
__O(t) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func isSubsequence(_ s: String, _ t: String) -> Bool {
        let s: [Character] = Array(s)
        var i: Int = 0

        // Use i to keep track of sequence in s
        // Iterate through t, if s[i] matches element in t, advance i
        for char in t where i < s.count && s[i] == char {
            i += 1

            // If i reaches end of s before we finish iterating through t
            // then s is a subsequence of t
            if i == s.count {
                return true
            }
        }
        return i == s.count
    }
}
```
__O(t+s\*log\[base 2\](t)) Time, O(t) Space - Binary-Search:__
```Swift
class Solution {
    func isSubsequence(_ s: String, _ t: String) -> Bool {
        var lookup : [Character: [Int]] = [:]
        for (index, value) in t.enumerated() {
            lookup[value, default: []].append(index)
        }
        var curr = -1
        for (index, value) in s.enumerated() {
            guard let indices = lookup[value] else { return false }
            switch binarySearch(indices, curr) {
                case -1:
                return false
                case let index:
                curr = index
            }
        }
        return true
    }
    
    func binarySearch(_ input: [Int], _ target: Int) -> Int {
        var start = 0, end = input.count-1
        while start+1 < end {
            let mid = start + (end-start)/2
            if input[mid] <= target {
                start = mid
            } else {
                end = mid
            }
        }
        if input[start] > target {
            return input[start]
        }
        if input[end] > target {
            return input[end]
        }
        return -1
    }
}
```