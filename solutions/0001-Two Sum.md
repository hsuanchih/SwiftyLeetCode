### Two Sum

Given an array of integers, return __indices__ of the two numbers such that they add up to a specific target.</br>
You may assume that each input would have *__exactly__* one solution, and you may not use the same element twice.

__Example:__
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
### Solution
__O(nums^2) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    // Try every combination
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        for i in stride(from: 0, to: nums.count, by: 1) {
            for j in stride(from: i+1, to: nums.count, by: 1) {
                if nums[i] + nums[j] == target {
                    return [i, j]
                }
            }
        }
        return []
    }
}
```
__O(nums) Time, O(nums) Space - HashMap:__
```Swift
class Solution {
    // Use hashtable to store visited [value:index] pair
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var indexLookup: [Int: Int] = [:]
        for index in 0 ..< nums.count {
            let num = nums[index], difference = target - num
            if let otherIndex = indexLookup[difference] {
                return [otherIndex, index]
            }
            indexLookup[num] = index
        }
        return []
    }
}
```
