
### Combinations

Given two integers `n` and `k`, return all possible combinations of `k` numbers chosen from the range `[1, n]`.

You may return the answer in __any order__.

__Example 1:__
```
Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.
```
__Example 2:__
```
Input: n = 1, k = 1
Output: [[1]]
Explanation: There is 1 choose 1 = 1 total combination.
```

__Constraints:__
* `1 <= n <= 20`
* `1 <= k <= n`

### Solution
__O(C(n,k)):__
```Swift
class Solution {
    func combine(_ n: Int, _ k: Int) -> [[Int]] {
        var temp: [Int] = []
        var result: [[Int]] = []
        generate(n, k, 1, &temp, &result)
        return result
    }

    func generate(_ n: Int, _ k: Int, _ i: Int, _ temp: inout [Int], _ result: inout [[Int]]) {
        if temp.count == k {
            result.append(temp)
        } else {
            for index in stride(from: i, through: n, by: 1) {
                temp.append(index)
                generate(n, k, index + 1, &temp, &result)
                temp.removeLast()
            }
        }
    }
}
```