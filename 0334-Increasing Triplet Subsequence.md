
### Increasing Triplet Subsequence

Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:
```
Return true if there exists i, j, k
such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.
```
__Note:__ Your algorithm should run in __O(n)__ time complexity and __O(1)__ space complexity.

__Example 1:__
```
Input: [1,2,3,4,5]
Output: true
```
__Example 2:__
```
Input: [5,4,3,2,1]
Output: false
```

### Solution
__O(nums^2) Time, O(nums) Space:__
```Swift
class Solution {
    func increasingTriplet(_ nums: [Int]) -> Bool {

        // This solution extends Problem 300: Longest Increasing Subsequence
        // Only it early returns when the length of longest increasing subsequence reaches 3
        if nums.count < 3 { return false }
        var memo : [Int] = Array(repeating: 1, count: nums.count)
        for end in 0..<nums.count {
            for index in 0...end {
                if nums[end] > nums[index] {
                    memo[end] = max(memo[end], memo[index]+1)
                }
                if memo[end] >= 3 {
                    return true
                }
            }
        }
        return false
    }
}
```
__O(nums) Time, O(1) Space:__
```Swift
class Solution {
    func increasingTriplet(_ nums: [Int]) -> Bool {
        if nums.count < 3 { return false }

        // Greedy approach:
        // Track the minimum & second minimum of nums' elements effective at index i,
        // return true when an element greater than the second minimum is found
        var min = Int.max, secondMin = min

        for num in nums {
            switch num {

                // If element is less than min, 
                // update min
                case (Int.min..<min):
                min = num

                // If element is greater than min but less than secondMin,
                // update secondMin
                case (min+1..<secondMin):
                secondMin = num
                
                // If element is greater than secondMin,
                // return true
                default:
                if num > secondMin {
                    return true
                }
            }
        }
        return false
    }
}
```