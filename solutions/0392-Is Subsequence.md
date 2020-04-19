
### Is Subsequence

Given a string __s__ and a string __t__, check if __s__ is subsequence of __t__.

You may assume that there is only lower case English letters in both __s__ and __t__.</br> 
__t__ is potentially a very long (length ~= 500,000) string, and __s__ is a short string (<=100).

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none)</br> 
of the characters without disturbing the relative positions of the remaining characters.</br> 
(ie, `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

__Example 1:__
```
s = "abc", t = "ahbgdc"

Return true.
```
__Example 2:__
```
s = "axc", t = "ahbgdc"

Return false.
```

__Follow up:__
If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B,</br> 
and you want to check one by one to see if T has its subsequence.</br> 
In this scenario, how would you change your code?

### Solution
__O(t) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func isSubsequence(_ s: String, _ t: String) -> Bool {
        if s.isEmpty { return true }
        let s = Array(s)
        var i = 0

        // Use i to keep track of sequence in s
        // Iterate through t, if s[i] matches element in t, advance i
        for char in t {
            if char == s[i] {
                i+=1
            }

            // If i reaches end of s before we finish iterating through t
            // then s is a subsequence of t
            if i == s.count {
                return true
            }
        }
        return false
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