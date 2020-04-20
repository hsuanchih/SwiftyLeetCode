
### Find All Numbers Disappeared in an Array

Given an array of integers where 1 ≤ a[i] ≤ *n* (*n* = size of array), some elements appear twice and others appear once.

Find all the elements of [1, *n*] inclusive that do not appear in this array.

Could you do it without extra space and in O(*n*) runtime? You may assume the returned list does not count as extra space.

__Example:__
```
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```

### Solution
__O(nums) Time, O(nums) Space:__
```Swift
class Solution {
    func findDisappearedNumbers(_ nums: [Int]) -> [Int] {
        guard !nums.isEmpty else { return nums }
        var set : Set<Int> = []
        for num in nums {
            set.insert(num)
        }
        var result : [Int] = []
        for num in 1...nums.count {
            if !set.contains(num) {
                result.append(num)
            }
        }
        return result
    }
}
```
__O(nums) Time, O(1) Space:__
```Swift
class Solution {
    func findDisappearedNumbers(_ nums: [Int]) -> [Int] {
        var nums = nums, i = 0, result : [Int] = []
        while i < nums.count {
            if i != nums[i]-1 && nums[i] != nums[nums[i]-1] {
                nums.swapAt(i, nums[i]-1)
            } else {
                i+=1
            }
        }
        for (index, value) in nums.enumerated() {
            if index != value-1 {
                result.append(index+1)
            }
        }
        return result
    }
}
```