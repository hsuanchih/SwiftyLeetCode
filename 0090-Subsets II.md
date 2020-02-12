
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
__Set:__
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
__Array:__
```Swift
class Solution {
    func subsetsWithDup(_ nums: [Int]) -> [[Int]] {
        var temp : [Int] = [], result : [[Int]] = []
        solve(nums.sorted(), 0, &temp, &result)
        return result
    }
    
    func solve(_ nums: [Int], _ index: Int, _ temp: inout [Int], _ result: inout [[Int]]) {
        result.append(temp)
        var index = index
        while index < nums.count {
            temp.append(nums[index])
            solve(nums, index+1, &temp, &result)
            temp.removeLast()
            while index < nums.count-1 && nums[index] == nums[index+1] {
                index+=1
            }
            index+=1
        }
    }
}
```