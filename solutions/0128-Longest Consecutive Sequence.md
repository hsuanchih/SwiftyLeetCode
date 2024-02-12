
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
__O(nums*log(nums)) Time - Sort Input:__
```Swift
class Solution {
    func longestConsecutive(_ nums: [Int]) -> Int {
        guard nums.count > 1 else { return nums.count }
        let nums = nums.sorted()
        var result: Int = 1
        var length: Int = 1
        for index in 1 ..< nums.count {
            switch nums[index] - nums[index - 1] {
            case 0:
                break
            case 1:
                length += 1
            default:
                result = max(result, length)
                length = 1
            }
        }
        return max(result, length)
    }
}
```
__O(nums) Time - HashSet:__
```Swift
class Solution {
    func longestConsecutive(_ nums: [Int]) -> Int {
        var lookup: Set<Int> = nums.reduce(into: Set<Int>()) { partialResult, num in
            partialResult.insert(num)
        }
        var result: Int = 0
        for num in lookup {
            var length: Int = 0
            var current: Int = num
            while lookup.contains(current) {
                lookup.remove(current)
                length += 1
                current -= 1
            }
            current = num + 1
            while lookup.contains(current) {
                lookup.remove(current)
                length += 1
                current += 1
            }
            result = max(result, length)
        }
        return result
    }
}
```