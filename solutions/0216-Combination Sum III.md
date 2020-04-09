
### Combination Sum III

Find all possible combinations of __k__ numbers that add up to a number __n__, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

__Note:__
* All numbers will be positive integers.
* The solution set must not contain duplicate combinations.

__Example 1:__
```
Input: k = 3, n = 7
Output: [[1,2,4]]
```
__Example 2:__
```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

### Solution
```Swift
class Solution {
    func combinationSum3(_ k: Int, _ n: Int) -> [[Int]] {
        var temp : [Int] = [], result : [[Int]] = []
        solve(k, n, 1, &temp, &result)
        return result
    }
    
    func solve(_ k: Int, _ n: Int, _ index: Int, _ temp: inout [Int], _ result: inout [[Int]]) {
        guard k > 0 else {
            if n == 0 {
                result.append(temp)
            }
            return
        }
        for num in stride(from: index, through: 9, by: 1) where num <= n {
            temp.append(num)
            solve(k-1, n-num, num+1, &temp, &result)
            temp.removeLast()
        }
    }
}
```