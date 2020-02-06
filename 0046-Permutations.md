
### Permutations

Given a collection of __distinct__ integers, return all possible permutations.

__Example:__
```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

### Solution
__O(n!*n):__
```Swift
class Solution {
    func permute(_ nums: [Int]) -> [[Int]] {
        var temp : [Int] = [], result : [[Int]] = []
        solve(nums, &temp, &result)
        return result
    }
    
    func solve(_ nums: [Int], _ temp: inout [Int], _ result: inout [[Int]]) {
        if temp.count == nums.count {
            result.append(temp)
            return
        }
        for num in nums {
            if !temp.contains(num) {
                temp.append(num)
                solve(nums,  &temp, &result)
                temp.removeLast()
            }
        }
    }
}
```
__O(n!):__
```Swift
class Solution {
    func permute(_ nums: [Int]) -> [[Int]] {
        var temp : [Int] = [], seen : Set<Int> = [], result : [[Int]] = []
        solve(nums, &seen, &temp, &result)
        return result
    }
    
    func solve(_ nums: [Int], _ seen: inout Set<Int>, _ temp: inout [Int], _ result: inout [[Int]]) {
        if temp.count == nums.count {
            result.append(temp)
            return
        }
        for num in nums {
            if !seen.contains(num) {
                seen.insert(num)
                temp.append(num)
                solve(nums, &seen, &temp, &result)
                temp.removeLast()
                seen.remove(num)
            }
        }
    }
}
```