
### Reverse Vowels of a String

Write a function that takes a string as input and reverse only the vowels of a string.

__Example 1:__
```
Input: "hello"
Output: "holle"
```
__Example 2:__
```
Input: "leetcode"
Output: "leotcede"
```

__Note:__
The vowels does not include the letter `"y"`.

### Solution
__O(s) Time:__
```Swift
class Solution {
    let vowels : Set<Character> = ["A", "E", "I", "O", "U", "a", "e", "i", "o", "u"]
    
    func reverseVowels(_ s: String) -> String {
        var s = Array(s)
        var start = 0, end = s.count-1
        while start < end {
            if !vowels.contains(s[start]) {
                start+=1
                continue
            }
            if !vowels.contains(s[end]) {
                end-=1
                continue
            }
            s.swapAt(start, end)
            start+=1
            end-=1
        }
        return String(s)
    }
}
```