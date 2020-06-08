
### Bulb Switcher

There are *n* bulbs that are initially off. You first turn on all the bulbs. Then, you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the *i*-th round, you toggle every *i* bulb. For the *n*-th round, you only toggle the last bulb. Find how many bulbs are on after *n* rounds.

__Example:__
```
Input: 3
Output: 1 
Explanation: 
At first, the three bulbs are [off, off, off].
After first round, the three bulbs are [on, on, on].
After second round, the three bulbs are [on, off, on].
After third round, the three bulbs are [on, off, off]. 

So you should return 1, because there is only one bulb is on.
```

### Solution
__O(n^2+n) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func bulbSwitch(_ n: Int) -> Int {
        if n == 0 {
            return n
        }
        var bulbs : [Bool] = Array(repeating: false, count: n+1)
        for round in 1...n {
            for bulb in 1...n {
                if bulb%round == 0 {
                    bulbs[bulb] = !bulbs[bulb]
                }
            }
        }
        return bulbs.reduce(0) { $1 == true ? $0+1 : $0 }
    }
}
```
__O(1) Time, O(1) Space - Math:__
```Swift
class Solution {
    func bulbSwitch(_ n: Int) -> Int {
        return Int(sqrt(Double(n)))
    }
}
```