
### Self Dividing Numbers

A *self-dividing number* is a number that is divisible by every digit it contains.

For example, 128 is a self-dividing number because `128 % 1 == 0`, `128 % 2 == 0`, and `128 % 8 == 0`.

Also, a self-dividing number is not allowed to contain the digit zero.

Given a lower and upper number bound, output a list of every possible self dividing number, including the bounds if possible.

__Example 1:__
```
Input: 
left = 1, right = 22
Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]
```

__Note:__
* The boundaries of each input argument are `1 <= left <= right <= 10000`.

### Solution
__O((right-left+1)*digits) Time:__
```Swift
class Solution {
    func selfDividingNumbers(_ left: Int, _ right: Int) -> [Int] {
        return Array(left...right).filter(isSelfDividing)
    }
    
    func isSelfDividing(_ num: Int) -> Bool {
        var temp = num
        while temp > 0 {
            let digit = temp%10
            if digit == 0 || num%digit != 0 {
                return false
            }
            temp/=10
        }
        return true
    }
}
```