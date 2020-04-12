
### Arranging Coins

You have a total of *n* coins that you want to form in a staircase shape, where every *k*-th row must have exactly *k* coins.

Given *n*, find the total number of __full__ staircase rows that can be formed.

*n* is a non-negative integer and fits within the range of a 32-bit signed integer.

__Example 1:__
```
n = 5

The coins can form the following rows:
¤
¤ ¤
¤ ¤

Because the 3rd row is incomplete, we return 2.
```
__Example 2:__
```
n = 8

The coins can form the following rows:
¤
¤ ¤
¤ ¤ ¤
¤ ¤

Because the 4th row is incomplete, we return 3.
```

### Solution
__O(n):__
```Swift
class Solution {
    func arrangeCoins(_ n: Int) -> Int {
        var num = n
        for row in 0...num {
            if num < row {
                return row-1
            }
            num-=row
        }
        return n
    }
}
```
__O(log(n)):__
```Swift
class Solution {
    func arrangeCoins(_ n: Int) -> Int {
        var start = 0, end = n
        while start+1 < end {
            let mid = start + (end-start)/2
            switch (1+mid)*mid/2 {
                case n:
                return mid
                case n+1...Int.max:
                end = mid
                default:
                start = mid
            }
        }
        return (1+end)*end/2 <= n ? end : start
    }
}
```