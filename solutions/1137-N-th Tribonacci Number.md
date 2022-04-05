
### N-th Tribonacci Number

The Tribonacci sequence Tn is defined as follows: 

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given `n`, return the value of Tn.

__Example 1:__
```
Input: n = 4
Output: 4
Explanation:
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4
```
__Example 2:__
```
Input: n = 25
Output: 1389537
```

__Constraints:__
* `0 <= n <= 37`
* The answer is guaranteed to fit within a 32-bit integer, ie. `answer <= 2^31 - 1`.

### Solution
__O(n) Time, O(n) Space:__
```swift
class Solution {
    func tribonacci(_ n: Int) -> Int {
        var memo : [Int] = Array(repeating: 0, count: n+1)
        for t in 0...n {
            switch t {
                case 0:
                break
                case 1, 2:
                memo[t] = 1
                default:
                memo[t] = memo[t-3]+memo[t-2]+memo[t-1]
            }
        }
        return memo.last ?? 0
    }
}
```
__O(n) Time, O(1) Space:__
```swift
class Solution {
    func tribonacci(_ n: Int) -> Int {
        guard n > 1 else { return n }
        var current: Int = 1, minus1: Int = 1, minus2: Int = 0
        for _ in (2 ..< n) {
            let next = current + minus1 + minus2
            minus2 = minus1
            minus1 = current
            current = next
        }
        return current
    }
}
```