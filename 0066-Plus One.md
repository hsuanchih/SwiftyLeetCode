
### Plus One

Given a __non-empty__ array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list,</br>
and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

__Example 1:__
```
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```
__Example 2:__
```
Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
```

### Solution
__O(n):__
```Swift
class Solution {
    func plusOne(_ digits: [Int]) -> [Int] {
        var digits = digits, carry = 1
        for index in stride(from: digits.count-1, through: 0, by: -1) {
            let sum = digits[index] + carry
            digits[index] = sum%10
            carry = sum/10
        }
        if carry != 0 {
            digits.insert(carry, at: 0)
        }
        return digits
    }
}
```