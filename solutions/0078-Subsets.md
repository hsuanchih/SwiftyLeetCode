
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
__O(nums*2^nums) Time, Recursive:__
```Swift
class Solution {
    func subsets(_ nums: [Int]) -> [[Int]] {
        var temp : [Int] = [], result : [[Int]] = []
        generate(nums, 0, &temp, &result)
        return result
    }
    
    func generate(_ nums: [Int], _ index: Int, _ temp: inout [Int], _ result: inout [[Int]]) {
        result.append(temp)
        if index == nums.count {
            return
        }
        for i in index..<nums.count {
            temp.append(nums[i])
            generate(nums, i+1, &temp, &result)
            temp.removeLast()
        }
    }
}
```