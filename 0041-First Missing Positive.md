
### First Missing Positive

Given an unsorted integer array, find the smallest missing positive integer.

__Example 1:__
```
Input: [1,2,0]
Output: 3
```
__Example 2:__
```
Input: [3,4,-1,1]
Output: 2
```
__Example 3:__
```
Input: [7,8,9,11,12]
Output: 1
```
__Note:__
Your algorithm should run in O(*n*) time and uses constant extra space.

### Solution
__O(nlogn+n) Time, O(1) Space:__
```Swift
class Solution {
    func firstMissingPositive(_ nums: [Int]) -> Int {
        let nums = nums.lazy.sorted().filter { $0 > 0 }
        var curr = 1
        for i in stride(from: 0, to: nums.count, by: 1) {
            if nums[i] != curr {
                return curr
            }
            if i < nums.count-1 && nums[i] != nums[i+1] {
                curr+=1
            }
        }
        return (nums.last ?? 0)+1
    }
}
```
__O(n) Time, O(n) Space:__
```Swift
class Solution {
    func firstMissingPositive(_ nums: [Int]) -> Int {
        var seen : Set<Int> = []
        for num in nums {
            seen.insert(num)
        }
        for index in stride(from: 1, through: nums.count, by: 1) {
            if !seen.contains(index) {
                return index
            }
        }
        return nums.count+1
    }
}
```
__O(n) Time, O(1) Space:__
```Swift
class Solution {
    func firstMissingPositive(_ nums: [Int]) -> Int {
        var nums = nums
        for i in stride(from: 0, to: nums.count, by: 1) {
            while nums[i] > 0 && nums[i] <= nums.count && nums[i] != nums[nums[i]-1] {
                nums.swapAt(i, nums[i]-1)
            }
        }
        for i in stride(from: 0, to: nums.count, by: 1) {
            if nums[i] != i+1 {
                return i+1
            }
        }
        return nums.count+1
    }
}
```