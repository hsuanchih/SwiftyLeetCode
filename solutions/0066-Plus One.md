
### Plus One

You are given a __large integer__ represented as an integer array `digits`, where each `digits[i]` is the `ith` digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading `0`'s.

Increment the large integer by one and return the resulting array of digits.

__Example 1:__
```
Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].
```
__Example 2:__
```
Input: digits = [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
Incrementing by one gives 4321 + 1 = 4322.
Thus, the result should be [4,3,2,2].
```
__Example 3:__
```
Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].
```

__Constraints:__
* `1 <= digits.length <= 100`
* `0 <= digits[i] <= 9`
* `digits` does not contain any leading `0`'s.

### Solution
__O(digits) Time:__
```Swift
class Solution {
    func plusOne(_ digits: [Int]) -> [Int] {
        var digits: [Int] = digits
        var carry: Int = 1
        for i in stride(from: digits.count - 1, through: 0, by: -1) {
            let num: Int = digits[i] + carry
            digits[i] = num % 10
            carry = num / 10
        }
        if carry > 0 {
            digits.insert(carry, at: 0)
        }
        return digits
    }
}
```