
### Jewels and Stones

You're given strings `J` representing the types of stones that are jewels, and `S` representing the stones you have.</br>
Each character in `S` is a type of stone you have.  You want to know how many of the stones you have are also jewels.

The letters in `J` are guaranteed distinct, and all characters in `J` and `S` are letters.</br>
Letters are case sensitive, so `"a"` is considered a different type of stone from `"A"`.

__Example 1:__
```
Input: J = "aA", S = "aAAbbbb"
Output: 3
```
__Example 2:__
```
Input: J = "z", S = "ZZ"
Output: 0
```

__Note:__
* `S` and `J` will consist of letters and have length at most 50.
* The characters in `J` are distinct.

### Solution
__O(S+J) Time:__
```Swift
class Solution {
    func numJewelsInStones(_ J: String, _ S: String) -> Int {
        let J = Set(J)
        var result = 0
        for char in S {
            if J.contains(char) {
                result+=1
            }
        }
        return result
    }
}
```