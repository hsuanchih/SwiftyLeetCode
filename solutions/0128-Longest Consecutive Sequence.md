
### Longest Consecutive Sequence

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in `O(n)` time.

__Example 1:__
```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```
__Example 2:__
```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

__Constraints:__
* `0 <= nums.length <= pow(10, 5)`
* `-pow(10, 9) <= nums[i] <= pow(10, 9)`

### Solution
__O(pow(nums, 2)) Time - Set:__
```Swift
class Solution {
    func longestConsecutive(_ nums: [Int]) -> Int {
        guard !nums.isEmpty else { return 0 }
        var elements: Set<Int> = Set(nums)
        var result: Int = 1
        for num in nums {
            var longest: Int = 0
            var current: Int = num
            while elements.contains(current) {
                longest += 1
                current += 1
            }

            current = num - 1
            while elements.contains(current) {
                longest += 1
                current -= 1
            }
            result = max(result, longest)
        }
        return result
    }
}
```
__O(nums * log(nums)) Time - Sort Input:__
```Swift
class Solution {
    func longestConsecutive(_ nums: [Int]) -> Int {
        guard !nums.isEmpty else { return 0 }
        let nums: [Int] = nums.sorted()
        var longest: Int = 1
        var result: Int = 1
        for i in 1 ..< nums.count {
            switch nums[i] - nums[i - 1] {
            case 0:
                break
            case 1:
                longest += 1
            default:
                result = max(result, longest)
                longest = 1
            }
        }
        return max(result, longest)
    }
}
```
__O(nums) Time - Set + Dynamic Element Removal:__
```Swift
class Solution {
    func longestConsecutive(_ nums: [Int]) -> Int {
        guard !nums.isEmpty else { return 0 }
        var elements: Set<Int> = Set(nums)
        var result: Int = 1
        for num in nums {
            var longest: Int = 0
            var current: Int = num
            while elements.contains(current) {
                elements.remove(current)
                longest += 1
                current += 1
            }

            current = num - 1
            while elements.contains(current) {
                elements.remove(current)
                longest += 1
                current -= 1
            }
            result = max(result, longest)
        }
        return result
    }
}
```