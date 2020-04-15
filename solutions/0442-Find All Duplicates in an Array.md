
### Find All Duplicates in an Array

Given an array of integers, `1 ≤ a[i] ≤ n` (`n` = size of array), some elements appear __twice__ and others appear __once__.

Find all the elements that appear __twice__ in this array.

Could you do it without extra space and in O(n) runtime?

__Example:__
```
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
```

### Solution
__O(nums+unique(nums)) Time, O(unique(nums)) Space - HashMap:__
```Swift
class Solution {
    func findDuplicates(_ nums: [Int]) -> [Int] {
        let lookup = nums.reduce(into: [Int: Int]()) { $0[$1, default: 0]+=1 }
        return Array(lookup.filter { $0.value == 2 }.keys)
    }
}
```
__O(nums*2) Time, O(1) Space - Pigeon-Hole:__
```Swift
class Solution {
    func findDuplicates(_ nums: [Int]) -> [Int] {
        var nums = nums, result : [Int] = []
        var i = 0
        while i < nums.count {
            if nums[i] != i+1 && nums[nums[i]-1] != nums[i] {
                nums.swapAt(i, nums[i]-1)
            } else {
                i+=1
            }
        }
        for i in 0..<nums.count {
            if nums[i] != i+1 {
                result.append(nums[i])
            }
        }
        return result
    }
}
```
__O(nums+result) Time, O(1) Space - Pigeon-Hole Optimized:__
```Swift
class Solution {
    func findDuplicates(_ nums: [Int]) -> [Int] {
        var nums = nums, result : Set<Int> = []
        var i = 0
        while i < nums.count {
            if nums[i] != i+1 {
                if nums[nums[i]-1] == nums[i] {
                    result.insert(nums[i])
                    i+=1
                } else {
                    nums.swapAt(i, nums[i]-1)
                }
            } else {
                i+=1
            }
        }
        return Array(result)
    }
}
```