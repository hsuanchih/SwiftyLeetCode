
### Combination Sum

Given a __set__ of candidate numbers (candidates) __(without duplicates)__ and a target number (target),</br> 
find all unique combinations in candidates where the candidate numbers sums to target.

The __same__ repeated number may be chosen from candidates unlimited number of times.

__Note:__

* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.

__Example 1:__
```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```
__Example 2:__
```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

### Solution
__O(2^n):__
```Swift
class Solution {
    func combinationSum(_ candidates: [Int], _ target: Int) -> [[Int]] {
        var temp : [Int] = [], result : [[Int]] = []
        findSum(candidates, 0, target, &temp, &result)
        return result
    }
    
    func findSum(_ candidates: [Int], _ index: Int, _ target: Int, _ temp: inout [Int], _ result: inout [[Int]]) {
        if target == 0 {
            result.append(temp)
            return
        }
        for i in stride(from: index, to: candidates.count, by: 1) {
            let num = candidates[i]
            if target >= num {
                temp.append(num)
                findSum(candidates, i, target-num, &temp, &result)
                temp.removeLast()
            }
        }
    }
}
```