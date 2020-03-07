
### Find the Duplicate Number

Given an array *nums* containing *n* + 1 integers where each integer is between 1 and *n* (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

__Example 1:__
```
Input: [1,3,4,2,2]
Output: 2
```
__Example 2:__
```
Input: [3,1,3,4,2]
Output: 3
```

__Note:__
1. You __must not__ modify the array (assume the array is read only).
2. You must use only constant, O(1) extra space.
3. Your runtime complexity should be less than O(n^2).
4. There is only one duplicate number in the array, but it could be repeated more than once.

### Solution
__O(n^2) Time, O(1) Space:__
```Swift
class Solution {
    func findDuplicate(_ nums: [Int]) -> Int {
        for start in 0..<nums.count {
            for i in start+1..<nums.count {
                if nums[i] == nums[start] {
                    return nums[i]
                }
            }
        }
        return -1
    }
}
```
__O(n) Time, O(n) Space:__
```Swift
class Solution {
    func findDuplicate(_ nums: [Int]) -> Int {
        var set : Set<Int> = []
        for num in nums {
            if set.contains(num) {
                return num
            }
            set.insert(num)
        }
        return -1
    }
}
```
__O(n) Time, O(1) Space:__
```Swift
class Solution {
    func findDuplicate(_ nums: [Int]) -> Int {
        var index = 0, nums = nums

        // Swap elements into their corresponding indices in nums,
        // ie, [1,2,2,3,4]
        while index < nums.count {
            if nums[index]-1 != index && nums[index] != nums[nums[index]-1] {
                nums.swapAt(index, nums[index]-1)
                continue
            }
            index+=1
        }

        // Iterate through nums to identify the element that doesn't
        // match its corresponding index in nums, this is the duplicate number.
        for (index, num) in nums.enumerated() {
            if num != index+1 {
                return num
            }
        }
        return -1
    }
}
```