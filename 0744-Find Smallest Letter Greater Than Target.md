
### Find Smallest Letter Greater Than Target

Given a list of sorted characters `letters` containing only lowercase letters, and given a target letter `target`,</br> 
find the smallest element in the list that is larger than the given target.

Letters also wrap around. For example, if the target is `target = 'z'` and `letters = ['a', 'b']`, the answer is `'a'`.

__Examples:__
```
Input:
letters = ["c", "f", "j"]
target = "a"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "c"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "d"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "g"
Output: "j"

Input:
letters = ["c", "f", "j"]
target = "j"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "k"
Output: "c"
```

__Note:__
1. `letters` has a length in range `[2, 10000]`.
2. `letters` consists of lowercase letters, and contains at least 2 unique letters.
3. `target` is a lowercase letter.

### Solution
__O(log(n)):__
```Swift
class Solution {
    func nextGreatestLetter(_ letters: [Character], _ target: Character) -> Character {
        var start : Int = 0, end : Int = letters.count-1
        while start+1 < end {
            let mid = start + (end-start)/2
            switch Int(letters[mid].asciiValue!) - Int(target.asciiValue!) {
                case Int.min ... 0:
                start = mid
                default:
                end = mid
            }
        }
        switch Int(letters[end].asciiValue!)-Int(target.asciiValue!) {
            case Int.min ... 0:
            return letters[(end+1)%letters.count]
            default:
            return letters[start].asciiValue! > target.asciiValue! ? letters[start] : letters[end]
        }
    }
}
```