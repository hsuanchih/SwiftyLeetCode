
### Find Smallest Letter Greater Than Target

You are given an array of characters `letters` that is sorted in non-decreasing order, and a character `target`.</br> 
There are __at least two different__ characters in `letters`.

Return _the smallest character in_ `letters` _that is lexicographically greater than_ `target`. If such a character does not exist, return the first character in `letters`.

__Example 1:__
```
Input: letters = ["c","f","j"], target = "a"
Output: "c"
Explanation: The smallest character that is lexicographically greater than 'a' in letters is 'c'.
```
__Example 2:__
```
Input: letters = ["c","f","j"], target = "c"
Output: "f"
Explanation: The smallest character that is lexicographically greater than 'c' in letters is 'f'.
```
__Example 3:__
```
Input: letters = ["x","x","y","y"], target = "z"
Output: "x"
Explanation: There are no characters in letters that is lexicographically greater than 'z' so we return letters[0].
```

__Constraints:__
* `2 <= letters.length <= 10^4`
* `letters[i]` is a lowercase English letter.
* `letters` is sorted in __non-decreasing__ order.
* `letters` contains at least two different characters.
* `target` is a lowercase English letter.

### Solution
__O(letters) Time - Linear Search:__
```Swift
class Solution {
    func nextGreatestLetter(_ letters: [Character], _ target: Character) -> Character {
        for letter in letters {
            if letter > target {
                return letter
            }
        }
        return letters.first!
    }
}
```
__O(log\[base 2\](letters)) Time - Binary Search:__
```Swift
class Solution {
    func nextGreatestLetter(_ letters: [Character], _ target: Character) -> Character {
        var start: Int = 0, end: Int = letters.count - 1
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            if letters[mid] > target {
                end = mid
            } else {
                start = mid
            }
        }
        if letters[start] > target {
            return letters[start]
        } else if letters[end] > target {
            return letters[end]
        } else {
            return letters.first!
        }
    }
}
```