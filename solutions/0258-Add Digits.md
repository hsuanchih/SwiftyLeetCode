
### Add Digits

Given a non-negative integer `num`, repeatedly add all its digits until the result has only one digit.

__Example:__
```
Input: 38
Output: 2 
Explanation: The process is like: 3 + 8 = 11, 1 + 1 = 2. 
             Since 2 has only one digit, return it.
```

__Follow up:__
Could you do it without any loop/recursion in O(1) runtime?

### Solution
__Recursive:__
```Swift
class Solution {
    func addDigits(_ num: Int) -> Int {
        if num/10 == 0 {
            return num
        }
        var num = num, sum = 0
        while num > 0 {
            sum+=num%10
            num/=10
        }
        return addDigits(sum)
    }
}
```
__Iterative:__
```Swift
class Solution {
    func addDigits(_ num: Int) -> Int {
        var num = num
        while num >= 10 {
            var curr = num, sum = 0
            while curr > 0 {
                sum+=curr%10
                curr/=10
            }
            num = sum
        }
        return num
    }
}
```