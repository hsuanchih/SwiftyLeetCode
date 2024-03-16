
### Subsets

Given a set of __distinct__ integers, *nums*, return all possible subsets (the power set).

__Note:__ The solution set must not contain duplicate subsets.

__Example:__
```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

### Solution
__O(pow(2, nums)) Time - Recursive:__
```Swift
class Solution {
    func subsets(_ nums: [Int]) -> [[Int]] {
        var result: [[Int]] = []
        addSubset(nums, 0, [], &result)
        return result
    }

    func addSubset(_ nums: [Int], _ index: Int, _ temp: [Int], _ result: inout [[Int]]) {
        result.append(temp)
        guard index < nums.count else { return }
        for i in index ..< nums.count {
            addSubset(nums, i + 1, temp + [nums[i]], &result)
        }
    }
}
```