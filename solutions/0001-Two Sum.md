### Two Sum

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have __exactly one solution__, and you may not use the same element twice.

You can return the answer in any order.

__Example 1:__
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```
__Example 2:__
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```
__Example 3:__
```
Input: nums = [3,3], target = 6
Output: [0,1]
```

__Constraints:__
* `2 <= nums.length <= pow(10, 4)`
* `-pow(10, 9) <= nums[i] <= pow(10, 9)`
* `-pow(10, 9) <= target <= pow(10, 9)`
* Only one valid answer exists.

__Follow-up:__ 
* Can you come up with an algorithm that is less than `O(pow(n, 2))` time complexity?

### Solution
__O(pow(nums, 2)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    // Try every combination
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        guard !nums.isEmpty else { return [] }
        for anchor in 0 ..< nums.count - 1 {
            for other in anchor + 1 ..< nums.count where nums[anchor] + nums[other] == target {
                return [anchor, other]
            }
        }
        return []
    }
}
```
__O((nums * log(nums)) + (2 * nums)) Time, O(nums) Space - Input Sorted:__
```Swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var indexLookup: [Int: [Int]] = nums
            .enumerated()
            .reduce(into: [Int: [Int]]()) { result, indexValue in
                let (index, value) = indexValue
                result[value, default: []].append(index)
            }
        let nums: [Int] = nums.sorted()

        var start: Int = 0, end: Int = nums.count - 1
        while start < end {
            switch nums[start] + nums[end] {
            case target:
                var result: [Int] = []
                [start, end].forEach {
                    if var indices = indexLookup[nums[$0]], !indices.isEmpty {
                        result.append(indices.removeFirst())
                        indexLookup[nums[$0]] = indices
                    }
                }
                return result
            case ..<target:
                start += 1
            case (target + 1)...:
                end -= 1
            default:
                fatalError()
            }
        }
        return []
    }
}
```
__O(nums) Time, O(nums) Space - HashMap:__
```Swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var indexLookupByNum: [Int: Int] = [:]
        for i in 0 ..< nums.count {
            let num: Int = nums[i]
            if let otherIndex = indexLookupByNum[target - num] {
                return [otherIndex, i]
            } else {
                indexLookupByNum[num] = i
            }
        }
        fatalError()
    }
}
```
