
### Max Consecutive Ones

Given a binary array, find the maximum number of consecutive 1s in this array.

__Example 1:__
```
Input: [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s.
    The maximum number of consecutive 1s is 3.
```

__Note:__
* The input array will only contain `0` and `1`.
* The length of input array is a positive integer and will not exceed 10,000

### Solution
```Swift
class Solution {
    func findMaxConsecutiveOnes(_ nums: [Int]) -> Int {
        var count = 0, result = 0
        for num in nums {
            if num == 0 {
                result = max(result, count)
                count = 0
            } else {
                count+=1
            }
        }
        return max(result, count)
    }
}
```