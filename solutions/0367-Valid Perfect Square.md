
### Valid Perfect Square

Given a positive integer *num*, write a function which returns True if *num* is a perfect square else False.

__Note:__ __Do not__ use any built-in library function such as `sqrt`.

__Example 1:__
```
Input: 16
Output: true
```
__Example 2:__
```
Input: 14
Output: false
```

### Solution
__O(num) Time - Brute-Force:__
```Swift
class Solution {
    func isPerfectSquare(_ num: Int) -> Bool {
        for i in 0..<num {
            switch i*i {
                case num:
                return true
                case num+1...Int.max:
                return false
                default:
                break
            }
        }
        return true
    }
}
```
__O(log\[\base 2\](num)) Time - Binary-Search:__
```Swift
class Solution {
    func isPerfectSquare(_ num: Int) -> Bool {
        if num <= 1 {
            return true
        }
        var start = 2, end = num/2
        while start+1 < end {
            let mid = start + (end-start)/2
            switch mid*mid {
                case num:
                return true
                case 2...num-1:
                start = mid
                default:
                end = mid
            }
        }
        return start*start == num || end*end == num
    }
}
```