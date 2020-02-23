
### Largest Perimeter Triangle

Given an array `A` of positive lengths, return the largest perimeter of a triangle with __non-zero area__, formed from 3 of these lengths.

If it is impossible to form any triangle of non-zero area, return `0`.

__Example 1:__
```
Input: [2,1,2]
Output: 5
```
__Example 2:__
```
Input: [1,2,1]
Output: 0
```
__Example 3:__
```
Input: [3,2,3,4]
Output: 10
```
__Example 4:__
```
Input: [3,6,2,3]
Output: 8
```

__Note:__
1. `3 <= A.length <= 10000`
2. `1 <= A[i] <= 10^6`

### Solution
__O(n*log(n)+n):__
```Swift
class Solution {
    func largestPerimeter(_ A: [Int]) -> Int {
        let A = A.sorted()
        for i in stride(from: A.count-3, through: 0, by: -1) {
            if A[i] + A[i+1] > A[i+2] {
                return A[i] + A[i+1] + A[i+2]
            }
        }
        return 0
    }
}
```