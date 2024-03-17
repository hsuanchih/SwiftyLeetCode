
### Subsets II

Given a collection of integers that might contain duplicates, *nums*, return all possible subsets (the power set).

__Note:__ The solution set must not contain duplicate subsets.

__Example:__
```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

### Solution
__O(nums * log(nums) + pow(2, nums)) Time, HashSet:__
```Swift
class Solution {
    func subsetsWithDup(_ nums: [Int]) -> [[Int]] {
        var temp : [Int] = [], result : Set<[Int]> = []
        solve(nums.sorted(), 0, &temp, &result)
        return Array(result)
    }
    
    func solve(_ nums: [Int], _ index: Int, _ temp: inout [Int], _ result: inout Set<[Int]>) {
        result.insert(temp)
        for i in stride(from: index, to: nums.count, by: 1) {
            temp.append(nums[i])
            solve(nums, i+1, &temp, &result)
            temp.removeLast()
        }
    }
}
```
__O(nums * log(nums) + pow(2, nums)) Time:__
```Swift
class Solution {
    func subsetsWithDup(_ nums: [Int]) -> [[Int]] {
        var result: [[Int]] = []
        genSubsets(nums.sorted(), 0, [], &result)
        return result
    }

    func genSubsets(_ nums: [Int], _ index: Int, _ temp: [Int], _ result: inout [[Int]]) {
        result.append(temp)
        var currentNum: Int?
        for i in index ..< nums.count where nums[i] != currentNum {
            currentNum = nums[i]
            genSubsets(nums, i + 1, temp + [nums[i]], &result)
        }
    }
}
```