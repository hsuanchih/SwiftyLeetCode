
### DI String Match

Given a string `S` that __only__ contains "I" (increase) or "D" (decrease), let `N = S.length`.

Return __any__ permutation `A` of `[0, 1, ..., N]` such that for all `i = 0, ..., N-1`:

* If `S[i] == "I"`, then `A[i] < A[i+1]`
* If `S[i] == "D"`, then `A[i] > A[i+1]` 

__Example 1:__
```
Input: "IDID"
Output: [0,4,1,3,2]
```
__Example 2:__
```
Input: "III"
Output: [0,1,2,3]
```
__Example 3:__
```
Input: "DDI"
Output: [3,2,0,1]
```

Note:
1. `1 <= S.length <= 10000`
2. `S` only contains characters `"I"` or `"D"`.

### Solution
__O(n):__
```Swift
class Solution {
    func diStringMatch(_ S: String) -> [Int] {
        var start = 0, end = S.count, A = Array(repeating: 0, count: S.count+1), i = 0
        for char in S {
            if char == "I" {
                A[i] = start
                start+=1
            } else {
                A[i] = end
                end-=1
            }
            i+=1
        }
        A[i] = start
        return A
    }
}
```