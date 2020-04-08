
### Permutations II

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

__Example:__
```
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

### Solution
__O(nums!) Time - Result is a HashSet:__
```Swift
class Solution {
    func permuteUnique(_ nums: [Int]) -> [[Int]] {
        var used : Set<Int> = [], temp : [Int] = [], result : Set<[Int]> = []
        solve(nums, &used, &temp, &result)
        return Array(result)
    }
    
    func solve(_ nums: [Int], _ used: inout Set<Int>, _ temp: inout [Int], _ result: inout Set<[Int]>) {
        if temp.count == nums.count {
            result.insert(temp)
            return
        }
        for index in 0..<nums.count {
            if !used.contains(index) {
                used.insert(index)
                temp.append(nums[index])
                solve(nums, &used, &temp, &result)
                temp.removeLast()
                used.remove(index)
            }
        }
    }
}
```
__O(nums*log\[\base 2]nums+nums!) Time - Sort Input:__
```Swift
class Solution {
    func permuteUnique(_ nums: [Int]) -> [[Int]] {
        var used : Set<Int> = [], temp : [Int] = [], result : [[Int]] = []
        solve(nums.sorted(), &used, &temp, &result)
        return result
    }
    
    func solve(_ nums: [Int], _ used: inout Set<Int>, _ temp: inout [Int], _ result: inout [[Int]]) {
        if temp.count == nums.count {
            result.append(temp)
            return
        }
        var index = 0
        while index < nums.count {
            defer { index+=1 }
            if used.contains(index) {
                continue
            }
            used.insert(index)
            temp.append(nums[index])
            solve(nums, &used, &temp, &result)
            temp.removeLast()
            used.remove(index)

            // Since nums is sorted, skip repeated elements to avoid duplicated results
            while index < nums.count-1 && nums[index] == nums[index+1] {
                index+=1
            }
        }
    }
}
```