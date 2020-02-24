
### Positions of Large Groups

In a string `S` of lowercase letters, these letters form consecutive groups of the same character.

For example, a string like `S = "abbxxxxzyy"` has the groups `"a"`, `"bb"`, `"xxxx"`, `"z"` and `"yy"`.

Call a group *large* if it has 3 or more characters.  We would like the starting and ending positions of every large group.

The final answer should be in lexicographic order.

__Example 1:__
```
Input: "abbxxxxzzy"
Output: [[3,6]]
Explanation: "xxxx" is the single large group with starting  3 and ending positions 6.
```
__Example 2:__
```
Input: "abc"
Output: []
Explanation: We have "a","b" and "c" but no large group.
```
__Example 3:__
```
Input: "abcdddeeeeaabbbcd"
Output: [[3,5],[6,9],[12,14]]
```

__Note:__  `1 <= S.length <= 1000`

### Solution
__O(S):__
```Swift
class Solution {
    func largeGroupPositions(_ S: String) -> [[Int]] {
        let S = Array(S)
        var result : [[Int]] = [], i = 0
        for j in 0..<S.count {
            if j == S.count-1 || S[i] != S[j+1] {
                if j-i+1 >= 3 {
                    result.append([i, j])
                }
                i = j+1
            }
        }
        return result
    }
}
```