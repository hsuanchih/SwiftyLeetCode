
### Contains Duplicate

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array,</br> 
and it should return false if every element is distinct.

__Example 1:__
```
Input: [1,2,3,1]
Output: true
```
__Example 2:__
```
Input: [1,2,3,4]
Output: false
```
__Example 3:__
```
Input: [1,1,1,3,3,4,3,2,4,2]
Output: true
```

### Solution
__O(nums^2) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        for i in 0..<nums.count {
            for j in i+1..<nums.count {
                if nums[i] == nums[j] {
                    return true
                }
            }
        }
        return false
    }
}
```
__O(nums) Time, O(nums) Space - HashSet:__
```Swift
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        var set : Set<Int> = []
        for num in nums {
            if set.contains(num) {
                return true
            }
            set.insert(num)
        }
        return false
    }
}
```