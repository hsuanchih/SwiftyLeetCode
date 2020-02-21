
### Find Numbers with Even Number of Digits

Given an array `nums` of integers, return how many of them contain an __even number__ of digits.

__Example 1:__
```
Input: nums = [12,345,2,6,7896]
Output: 2
Explanation: 
12 contains 2 digits (even number of digits). 
345 contains 3 digits (odd number of digits). 
2 contains 1 digit (odd number of digits). 
6 contains 1 digit (odd number of digits). 
7896 contains 4 digits (even number of digits). 
Therefore only 12 and 7896 contain an even number of digits.
```
__Example 2:__
```
Input: nums = [555,901,482,1771]
Output: 1 
Explanation: 
Only 1771 contains an even number of digits.
```

__Constraints:__
* `1 <= nums.length <= 500`
* `1 <= nums[i] <= 10^5`

### Solution
__O(5*n):__
```Swift
class Solution {
    func findNumbers(_ nums: [Int]) -> Int {
        return nums.reduce(0) { $0 + (isEvenDigits($1) ? 1 : 0) }
    }
    
    func isEvenDigits(_ num: Int) -> Bool {
        var scale = 1, count = 0
        while num/scale > 0 {
            scale*=10
            count+=1
        }
        return count%2 == 0
    }
}
```
__O(5*n):__
```Swift
class Solution {
    func findNumbers(_ nums: [Int]) -> Int {
        return nums.reduce(0) { $0 + (isEvenDigits($1) ? 1 : 0) }
    }
    
    func isEvenDigits(_ num: Int) -> Bool {
        return String(num).count%2 == 0
    }
}
```