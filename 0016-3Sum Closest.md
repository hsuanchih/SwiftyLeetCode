
### 3Sum Closest

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`.</br>
Return the sum of the three integers. You may assume that each input would have exactly one solution.

__Example:__
```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

### Solution
__O(n^3):__
```Swift
class Solution {
    func threeSumClosest(_ nums: [Int], _ target: Int) -> Int {
        var difference : Int = Int.max

        // Evaluate all triplet combinations
        for i in stride(from: 0, to: nums.count-2, by: 1) {
            for j in stride(from: i+1, to: nums.count, by: 1) {
                for k in stride(from: j+1, to: nums.count, by: 1) {

                    // Update difference if absolute value of sum-target is smaller than current difference
                    if abs(nums[i]+nums[k]+nums[j]-target) < abs(difference) {
                        difference = nums[i]+nums[k]+nums[j]-target
                    }
                }
            }
        }
        return target+difference
    }
}
```
__O(nlogn)+O(n^2):__
```Swift
class Solution {
    func threeSumClosest(_ nums: [Int], _ target: Int) -> Int {

        // Sort input
        let nums = nums.sorted()
        var difference : Int = Int.max

        // Using one element as anchor, and compute result based on sorted array property
        for i in stride(from: 0, to: nums.count-2, by: 1) {
            var start = i+1, end = nums.count-1
            while start < end {
                let curr = nums[i] + nums[start] + nums[end] - target
                if abs(curr) < abs(difference) {
                    difference = curr
                }
                switch curr {
                    case 0:
                    return target
                    case Int.min..<0:
                    start+=1
                    default:
                    end-=1
                }
            }
        }
        return target + difference
    }
}
```
