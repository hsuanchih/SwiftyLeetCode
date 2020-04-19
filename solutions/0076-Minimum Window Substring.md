
### Minimum Window Substring

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

__Example:__
```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```
__Note:__
* If there is no such window in S that covers all characters in T, return the empty string "".
* If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

### Solution
__O(s^2+t) Time, O(t) Space:__
```Swift
class Solution {
    func minWindow(_ s: String, _ t: String) -> String {

        // Convert t into a lookup table with each character & its corresponding frequency
        let s = Array(s), t = t.reduce(into: [Character: Int]()) { $0[$1, default: 0]+=1 }
        var result : Range<Int> = 0..<s.count+1

        // Iterate over s from every starting point to find the shortest substring including
        // all characters from t
        for start in 0..<s.count {

            // Reset lookup table for t
            var lookup = t

            for i in start..<s.count {
                let char = s[i]

                if let count = lookup[char] {
                    if count > 1 {
                        lookup[char] = count-1
                    } else {
                        lookup.removeValue(forKey: char)
                    }
                }

                // If lookup is empty, we've exhausted all characters in t
                // check if this is a smaller length substring than previous results
                if lookup.isEmpty {
                    if i-start+1 < result.count {
                        result = start..<i+1
                    }
                    break
                }
            }
        }
        return result.count == s.count+1 ? "" : String(s[result])
    }
}
```
__O(s\*t) Time, O(2\*t) Space:__
```Swift
class Solution {
    func minWindow(_ s: String, _ t: String) -> String {
        let s = Array(s), t = t.reduce(into: [Character: Int]()) { $0[$1, default: 0]+=1 }
        var start = 0, end = 0, lookup : [Character: Int] = [:], result : Range<Int> = 0..<s.count+1
        while end <= s.count {
            if isValidWindow(t, lookup) {
                if end-start < result.count {
                    result = start..<end
                }
                let char = s[start]
                if let count = lookup[char] {
                    lookup[char] = count-1
                }
                start+=1
            } else {
                if end < s.count, let _ = t[s[end]] {
                    lookup[s[end], default: 0]+=1
                }
                end+=1
            }
        }
        return result.count == s.count+1 ? "" : String(s[result])
    }
    
    func isValidWindow(_ benchmark: [Character: Int], _ test: [Character: Int]) -> Bool {
        for (key, value) in benchmark {
            guard let testValue = test[key], testValue >= value else { return false }
        }
        return true
    }
}
```