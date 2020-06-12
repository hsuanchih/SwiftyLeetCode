
### Maximum Product Subarray

Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.

__Example 1:__
```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```
__Example 2:__
```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

### Solution
__O(nums^2) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func maxProduct(_ nums: [Int]) -> Int {
        var result = Int.min
        for start in 0..<nums.count {
            var product = 1
            for end in start..<nums.count {
                product*=nums[end]
                result = max(product, result)
            }
        }
        return result
    }
}
```
__O(nums) Time - O(1) Space - Prefix-Product/Memoization:__
```Swift
class Solution {
    func maxProduct(_ nums: [Int]) -> Int {
        var maxProduct = 1, minProduct = 1, result = Int.min
        for num in nums {
            let prefixMax = max(maxProduct*num, minProduct*num),
            prefixMin = min(maxProduct*num, minProduct*num)
            maxProduct = max(prefixMax, num)
            minProduct = min(prefixMin, num)
            result = max(result, maxProduct)
        }
        return result
    }
}
```