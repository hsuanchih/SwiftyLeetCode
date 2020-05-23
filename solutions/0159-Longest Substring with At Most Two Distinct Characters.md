
### Longest Substring with At Most Two Distinct Characters

Given a string, find the length of the longest substring T that contains at most 2 distinct characters.

__Example 1:__
```
Input: “eceba”
Output: 3
Explanation:
T is "ece" which its length is 3.
```
__Example 2:__
```
Input: “aaa”
Output: 3
```

### Solution
__O(n) Time, O(1) Space - HashMap:__
```Swift
class Solution {
    func lengthOfLongestSubstringTwoDistinct(_ s: String) -> Int {
        let s = Array(s)
        var lookup : [Character: Int] = [:], result : Int = 0
        var start = 0, end = 0
        while end < s.count {
            if let count = lookup[s[end]] {
                lookup[s[end]] = count+1
                end+=1
            } else {
                if lookup.count == 2 {
                    result = max(result, end-start)
                    if let count = lookup[s[start]], count > 1 {
                        lookup[s[start]] = count-1
                    } else {
                        lookup.removeValue(forKey: s[start])
                    }
                    start+=1
                } else {
                    lookup[s[end], default: 0]+=1
                    end+=1
                }
            }
        }
        return max(result, end-start)
    }
}
```