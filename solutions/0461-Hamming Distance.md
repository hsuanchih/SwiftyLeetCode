
### Hamming Distance

The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Given two integers `x` and `y`, calculate the Hamming distance.

__Note:__
0 ≤ `x`, `y` < 231.

__Example:__
```
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```

### Solution
```Swift
class Solution {
    func hammingDistance(_ x: Int, _ y: Int) -> Int {
        let z = x ^ y
        var result = 0
        for i in 0..<31 {
            result += z>>i & 1
        }
        return result
    }
}
```