
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
__O(pow(candidates, target)):__
```Swift
class Solution {
    func combinationSum(_ candidates: [Int], _ target: Int) -> [[Int]] {
        var temp: [Int] = []
        var result: [[Int]] = []
        findSum(candidates, target, 0, &temp, &result)
        return result
    }

    func findSum(_ candidates: [Int], _ target: Int, _ start: Int, _ temp: inout [Int], _ result: inout [[Int]]) {
        if target == 0 {

            // Base case: we have a solution
            // add the current combination to result
            result.append(temp)
        } else {

            // For each number in candidates starting at `start`
            // exhaustively search of a solution while pruning the search space.
            // We only want to continue the search if the current candidate is less than or equal
            // to `target`
            for i in start ..< candidates.count where target >= candidates[i] {
                temp.append(candidates[i])
                findSum(candidates, target - candidates[i], i, &temp, &result)
                temp.removeLast()
            }
        }
    }
}
```