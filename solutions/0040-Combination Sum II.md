
### Combination Sum

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

Each number in `candidates` may only be used __once__ in the combination.

__Note:__
* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.

__Example 1:__
```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```
__Example 2:__
```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```

### Solution
```
Condition to watch out for is duplicated results.

For example:
[1,2,7,6,1,5], target = 8 will yield results: 
[1,2,5] = [2,1,5]
[1,7] = [7,1]
[1,1,6]
[2,6]
```
__O(2^candidates) Time - Unsorted Input + Sorted Output + HashSet:__
```Swift
class Solution {
    func combinationSum2(_ candidates: [Int], _ target: Int) -> [[Int]] {
        var temp : [Int] = [], result : Set<[Int]> = []
        solve(candidates, target, 0, &temp, &result)
        return Array(result)
    }
    
    func solve(_ candidates: [Int], _ target: Int, _ index: Int, _ temp: inout [Int], _ result: inout Set<[Int]>) {
        if target == 0 {
            result.insert(temp.sorted())
            return
        }
        for i in stride(from: index, to: candidates.count, by: 1) {
            let num = candidates[i]
            if num <= target {
                temp.append(num)
                solve(candidates, target-num, i+1, &temp, &result)
                temp.removeLast()
            }
        }
    }
}
```
__O(2^candidates) Time - Sorted Input + Skip Repeating Consecutive Input:__
```Swift
class Solution {
    func combinationSum2(_ candidates: [Int], _ target: Int) -> [[Int]] {
        var temp : [Int] = [], result : [[Int]] = []
        solve(candidates.sorted(), target, 0, &temp, &result)
        return result
    }
    
    func solve(_ candidates: [Int], _ target: Int, _ index: Int, _ temp: inout [Int], _ result: inout [[Int]]) {
        if target == 0 {
            result.append(temp)
            return
        }
        var i = index
        while i < candidates.count {
            let num = candidates[i]
            if num > target {
                break
            }
            temp.append(num)
            solve(candidates, target-num, i+1, &temp, &result)
            temp.removeLast()
            while i < candidates.count-1 && num == candidates[i+1] {
                i+=1
            }
            i+=1
        }
    }
}
```