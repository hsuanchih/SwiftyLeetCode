
### Partition Equal Subset Sum

Given a __non-empty__ array containing __only positive integers__,</br> 
find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

__Note:__
```
1. Each of the array element will not exceed 100.
2. The array size will not exceed 200.
```

__Example 1:__
```
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```
__Example 2:__
```
Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.
```

### Solution
__Recursive O(2^n):__
```Swift
class Solution {
    func canPartition(_ nums: [Int]) -> Bool {
        return partition(nums, 0, nums.reduce(0) { $0+$1 }, 0)
    }
    
    func partition(_ nums: [Int], _ index: Int, _ total: Int, _ sum: Int) -> Bool {
        if index == nums.count {
            return total-sum == sum
        }
        return partition(nums, index+1, total, sum) || partition(nums, index+1, total, sum+nums[index])
    }
}
```
__Recursive Optimized O(2^(n/2)):__
```Swift
class Solution {
    func canPartition(_ nums: [Int]) -> Bool {
        let sum = nums.reduce(0) { $0+$1 }

        // If there are 2 different subsets of the same sum, then sum must be
        // divisible by 2
        if sum%2 != 0 {
            return false
        }

        // If any of the subsets add up to sum/2, then we have a solution
        return partition(nums, 0, sum/2)
    }
    
    func partition(_ nums: [Int], _ index: Int, _ sum: Int) -> Bool {
        if index == nums.count {
            return false
        }
        switch sum {
            case 0:
            return true
            case Int.min..<0:
            return false
            default:
            break
        }
        if partition(nums, index+1, sum-nums[index]) {
            return true
        }
        return partition(nums, index+1, sum)
    }
}
```
__Recursive Top-Down O(n\*sum) Time, O(n*sum) Space:__
```Swift
class Solution {
    func canPartition(_ nums: [Int]) -> Bool {
        let sum = nums.reduce(0) { $0+$1 }
        if sum%2 != 0 {
            return false
        }
        var memo : [[Bool?]] = Array(repeating: Array(repeating: nil, count: sum/2+1), count: nums.count)
        return partition(nums, 0, sum/2, &memo)
    }
    
    func partition(_ nums: [Int], _ index: Int, _ sum: Int, _ memo: inout [[Bool?]]) -> Bool {
        if index == nums.count {
            return false
        }
        switch sum {
            case 0:
            return true
            case Int.min..<0:
            return false
            default:
            break
        }
        if let result = memo[index][sum] {
            return result
        }
        if partition(nums, index+1, sum-nums[index], &memo) {
            memo[index][sum] = true
        } else {
            memo[index][sum] = partition(nums, index+1, sum, &memo)
        }
        return memo[index][sum]!
    }
}
```
__Iterative Bottom-Up O(n\*sum) Time, O(n*sum) Space:__
```Swift
class Solution {
    func canPartition(_ nums: [Int]) -> Bool {
        let total = nums.reduce(0) { $0+$1 }
        if total%2 != 0 {
            return false
        }
        
        // Can we construct a specific sum using numbers up to index?
        var memo : [[Bool]] = Array(repeating: Array(repeating: false, count: total/2+1), count: nums.count)
        for index in 0 ..< nums.count {
            for sum in 0 ... total/2 {
                
                // We can construct sum = 0 using any of the subsets from
                // nums, the subset where we don't include any numbers
                if sum == 0 {
                    memo[index][sum] = true
                    continue
                }
                
                // If there is only one number (number at index 0), we
                // can construct a specific sum only if the number is
                // equal to the sum
                if index == 0 {
                    memo[index][sum] = nums[index] == sum
                    continue
                }
                
                // If we can construct sum from any subset up to
                // index-1, then surely we can construct the same sum
                // using subset up to index (by not including the 
                // number at index)
                
                // Otherwise if we can construct sum-nums[index] using
                // subsets up to index-1, then we can certainly construct
                // sum by including the nums[index]
                if memo[index-1][sum] {
                    memo[index][sum] = memo[index-1][sum]
                } else if sum-nums[index] >= 0 {
                    memo[index][sum] = memo[index-1][sum-nums[index]]
                } else {
                    memo[index][sum] = false
                }
            }
        }
        return memo[nums.count-1][total/2]
    }
}
```