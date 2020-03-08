
### Ugly Number II

Write a program to find the `n`-th ugly number.

Ugly numbers are __positive numbers__ whose prime factors only include `2, 3, 5`. 

__Example:__
```
Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
```

__Note:__
1. `1` is typically treated as an ugly number.
2. `n` does not exceed __1690__.

### Solution
__Brute Force:__
```Swift
class Solution {
    func nthUglyNumber(_ n: Int) -> Int {
        if n <= 3 {
            return n
        }
        var count = 3, num = 3
        while count < n {
            num+=1
            if isUgly(num) {
                count+=1
            }
        }
        return num
    }

    func isUgly(_ num: Int) -> Bool {
        switch num {
            case Int.min...0:
            return false
            case 1:
            return true
            default:
            for factor in [2,3,5] {
                if num%factor == 0 {
                    return isUgly(num/factor)
                }
            }
        }
        return false
    }
}
```
__Optimized:__
```Swift
class Solution {
    func nthUglyNumber(_ n: Int) -> Int {
        var result : [Int] = [1]
        var i = 0, j = 0, k = 0
        while result.count < n {
            let two = result[i]*2, three = result[j]*3, five = result[k]*5,
            next = min(min(two, three), five)
            if next == two {
                i+=1
            }
            if next == three {
                j+=1
            }
            if next == five {
                k+=1
            }
            result.append(next)
        }
        return result.last!
    }
}
```