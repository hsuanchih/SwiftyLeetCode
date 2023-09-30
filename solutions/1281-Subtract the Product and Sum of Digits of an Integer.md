
### Subtract the Product and Sum of Digits of an Integer

Given an integer number `n`, return the difference between the product of its digits and the sum of its digits.

__Example 1:__
```
Input: n = 234
Output: 15 
Explanation: 
Product of digits = 2 * 3 * 4 = 24 
Sum of digits = 2 + 3 + 4 = 9 
Result = 24 - 9 = 15
```
__Example 2:__
```
Input: n = 4421
Output: 21
Explanation: 
Product of digits = 4 * 4 * 2 * 1 = 32 
Sum of digits = 4 + 4 + 2 + 1 = 11 
Result = 32 - 11 = 21
```

__Constraints:__
* `1 <= n <= 10^5`

### Solution
__Time Linear to Digits in n:__
```Swift
class Solution {
    func subtractProductAndSum(_ n: Int) -> Int {
        var n: Int = n
        var sum: Int = 0
        var product: Int = 1
        while n > 0 {
            let digit: Int = n % 10
            sum += digit
            product *= digit
            n /= 10
        }
        return product - sum
    }
}
```