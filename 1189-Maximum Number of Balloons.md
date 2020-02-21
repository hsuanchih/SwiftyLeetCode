
### Maximum Number of Balloons

Given a string `text`, you want to use the characters of `text` to form as many instances of the word __"balloon"__ as possible.

You can use each character in text at most once. Return the maximum number of instances that can be formed.

__Example 1:__
```
Input: text = "nlaebolko"
Output: 1
```
__Example 2:__
```
Input: text = "loonbalxballpoon"
Output: 2
```
__Example 3:__
```
Input: text = "leetcode"
Output: 0
```

__Constraints:__

* `1 <= text.length <= 10^4`
* `text` consists of lower case English letters only.

### Solution
__O(n):__
```Swift
class Solution {
    func maxNumberOfBalloons(_ text: String) -> Int {
        let input = text.reduce(into: [Character: Int]()) { $0[$1, default: 0]+=1 }
        var result = Int.max
        for char in "balon" {
            if let count = input[char] {
                switch char {
                    case "l", "o":
                    result = min(result, count/2)
                    default:
                    result = min(result, count)
                }
            } else {
                return 0
            }
        }
        return result
    }
}
```