
### Longest Palindrome

Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example `"Aa"` is not considered a palindrome here.

__Note:__
Assume the length of given string will not exceed 1,010.

__Example:__
```
Input:
"abccccdd"

Output:
7

Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
```

### Solution
__O(s+unique(s)) Time, O(unique(s)) Space:__
```Swift
class Solution {
    func longestPalindrome(_ s: String) -> Int {

        // Store the frequency of each character in s
        var lookup : [Character: Int] = [:]
        for char in s {
            lookup[char, default: 0]+=1
        }
        
        // Iterate through lookup
        // If frequency is even, add all occurences of the character to the palindrome string
        // otherwise add frequency-1 number of the character to the palindrome
        var result = 0
        for (key, value) in lookup {
            let singles = value%2
            if singles == 0 {
                lookup.removeValue(forKey: key)
            } else {
                lookup[key] = singles
            }
            result+=value/2*2
        }
        // If lookup is empty, then we don't have leftover singles to add to the palindrome
        // Otherwise add a single to the palindrome to make it one character longer
        return lookup.isEmpty ? result : result+1
    }
}
```