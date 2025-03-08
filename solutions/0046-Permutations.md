
### Permutations

Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in __any order__.

__Example 1:__
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```
__Example 2:__
```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```
__Example 3:__
```
Input: nums = [1]
Output: [[1]]
```

__Constraints:__
* `1 <= nums.length <= 6`
* `-10 <= nums[i] <= 10`
* All the integers of `nums` are __unique__.

### Solution
__O(nums! * nums) Time - Exhaustive Search:__
```Swift
class Solution {
    func permute(_ nums: [Int]) -> [[Int]] {
        var temp: [Int] = []
        var result: [[Int]] = []
        generate(nums, &temp, &result)
        return result
    }

    func generate(_ nums: [Int], _ temp: inout [Int], _ result: inout [[Int]]) {
        if temp.count == nums.count {
            result.append(temp)
        } else {
            for num in nums where !temp.contains(num) {
                temp.append(num)
                generate(nums, &temp, &result)
                temp.removeLast()
            }
        }
    }
}
```
__O(nums!) Time - Exhaustive Search + HashSet:__
```Swift
class Solution {
    func permute(_ nums: [Int]) -> [[Int]] {
        var used: Set<Int> = []
        var temp: [Int] = []
        var result: [[Int]] = []
        generate(nums, &used, &temp, &result)
        return result
    }

    func generate(_ nums: [Int], _ used: inout Set<Int>, _ temp: inout [Int], _ result: inout [[Int]]) {
        if temp.count == nums.count {
            result.append(temp)
        } else {
            for num in nums where !used.contains(num) {
                used.insert(num)
                temp.append(num)
                generate(nums, &used, &temp, &result)
                temp.removeLast()
                used.remove(num)
            }
        }
    }
}
```