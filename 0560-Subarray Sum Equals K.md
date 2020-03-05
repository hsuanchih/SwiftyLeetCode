
### Subarray Sum Equals K

Given an array of integers and an integer __k__,</br> 
you need to find the total number of continuous subarrays whose sum equals to __k__.

__Example 1:__
```
Input:nums = [1,1,1], k = 2
Output: 2
```

__Note:__
1. The length of the array is in range `[1, 20,000]`.
2. The range of numbers in the array is `[-1000, 1000]` and the range of the integer __k__ is `[-1e7, 1e7]`.

### Solution
__O(sum^3):__
```Swift
class Solution {
    func subarraySum(_ nums: [Int], _ k: Int) -> Int {
        var result = 0
        for start in 0..<nums.count {
            for end in start..<nums.count {

            	// For each subarray starting at start & ending at end
            	// check if the subarray sum is equal to 0
                var temp = k
                for i in start...end {
                    temp-=nums[i]
                }
                if temp == 0 {
                    result+=1
                }
            }
        }
        return result
    }
}
```
__O(nums^2):__
```Swift
class Solution {
    func subarraySum(_ nums: [Int], _ k: Int) -> Int {
        var result = 0
        for start in 0..<nums.count {
            var temp = k
            for end in start..<nums.count {
                temp-=nums[end]
                if temp == 0 {
                    result+=1
                }
            }
        }
        return result
    }
}
```
__O(nums):__
```Swift
class Solution {
    func subarraySum(_ nums: [Int], _ k: Int) -> Int {

    	// lookup stores the number of occurrences of each sum
    	// prefixSum stores the sum from the first to the current element
        var lookup : [Int: Int] = [0:1], prefixSum = 0, result = 0

        for i in 0..<nums.count {

        	// Update prefix sum
            prefixSum += nums[i]

            // If there exists a subarray sum where prefixSum - k = sum
            // then there exists at least one solution update result with
            // the number of ocurrences of such sum
            result += lookup[prefixSum-k] ?? 0

            // Increment the frequency of the current sum
            lookup[prefixSum, default: 0] += 1
        }
        return result
    }
}
```