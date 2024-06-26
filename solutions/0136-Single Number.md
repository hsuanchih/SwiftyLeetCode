
### Single Number

Given a __non-empty__ array of integers `nums`, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

__Example 1:__
```
Input: nums = [2,2,1]
Output: 1
```
__Example 2:__
```
Input: nums = [4,1,2,1,2]
Output: 4
```
__Example 3:__
```
Input: nums = [1]
Output: 1
```

__Constraints:__
* `1 <= nums.length <= 3 * pow(10, 4)`
* `-3 * pow(10, 4) <= nums[i] <= 3 * pow(10, 4)`
* Each element in the array appears twice except for one element which appears only once.

### Solution
__O(pow(nums, 2)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        for i in 0 ..< nums.count {
            var result: Int? = nums[i]
            for j in 0 ..< nums.count where j != i && nums[j] == nums[i] {
                result = nil
                break
            }
            if let result {
                return result
            }
        }
        fatalError()
    }
}
```
__O(nums) Time, O(nums) Space:__
```Swift
class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        var seen: Set<Int> = []
        for num in nums {
            if seen.contains(num) {
                seen.remove(num)
            } else {
                seen.insert(num)
            }
        }
        return seen.first!
    }
}
```
__O(nums) Time, O(1) Space:__
```Swift
class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        return nums.reduce(0) { $0 ^ $1 }
    }
}
```