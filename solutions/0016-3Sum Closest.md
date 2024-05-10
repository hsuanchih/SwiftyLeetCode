
### 3Sum Closest

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`.</br>
Return the sum of the three integers. You may assume that each input would have exactly one solution.

__Example:__
```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

### Solution
__O(pow(nums, 3)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func threeSumClosest(_ nums: [Int], _ target: Int) -> Int {
        guard nums.count >= 3 else { fatalError() }
        var difference: Int = .max

        // Evaluate all triplet combinations
        for i in 0 ..< nums.count - 2 {
            for j in i + 1 ..< nums.count - 1 {
                for k in j + 1 ..< nums.count {
                    let delta: Int = nums[i] + nums[j] + nums[k] - target

                    // Update difference if absolute value of sum-target is smaller than current difference
                    if abs(delta) < abs(difference) {
                        difference = delta
                    }
                }
            }
        }
        return target + difference
    }
}
```
__O(nums * log(nums) + pow(nums, 2)) Time, O(1) Space - Sorted Input + 2 Pointers:__
```Swift
class Solution {
    func threeSumClosest(_ nums: [Int], _ target: Int) -> Int {
        guard nums.count >= 3 else { fatalError() }

        // Sort input
        let nums: [Int] = nums.sorted()
        var difference: Int = .max

        // Using one element as anchor, and compute result based on sorted array property
        for i in 0 ..< nums.count - 2 {
            var j: Int = i + 1, k: Int = nums.count - 1
            while j < k {
                let delta: Int = nums[i] + nums[j] + nums[k] - target
                if abs(delta) < abs(difference) {
                    difference = delta
                }
                switch delta {
                case 0:
                    return target
                case ..<0:
                    j += 1
                case 1...:
                    k -= 1
                default:
                    fatalError()
                }
            }
        }
        return target + difference
    }
}
```
