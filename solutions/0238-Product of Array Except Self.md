
### Product of Array Except Self

Given an integer array `nums`, return an array answer such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is __guaranteed__ to fit in a __32-bit__ integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

__Example 1:__
```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```
__Example 2:__
```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

__Constraints:__
* `2 <= nums.length <= pow(10, 5)`
* `-30 <= nums[i] <= 30`
* The product of any prefix or suffix of `nums` is guaranteed to fit in a __32-bit__ integer.
 

__Follow up:__ 
Can you solve the problem in `O(1)` extra space complexity? (The output array does not count as extra space for space complexity analysis.)

### Solution
__O(pow(n, 2)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func productExceptSelf(_ nums: [Int]) -> [Int] {
        var answer: [Int] = Array(repeating: 1, count: nums.count)
        for i in 0 ..< nums.count {
            for j in 0 ..< nums.count where j != i {
                answer[i] *= nums[j]
            }
        }
        return answer
    }
}
```