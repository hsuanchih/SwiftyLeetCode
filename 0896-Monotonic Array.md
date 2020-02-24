
### Monotonic Array

An array is *monotonic* if it is either monotone increasing or monotone decreasing.

An array A is monotone increasing if for all `i <= j`, `A[i] <= A[j]`.  An array `A` is monotone decreasing if for all `i <= j`, `A[i] >= A[j]`.

Return `true` if and only if the given array `A` is monotonic.

__Example 1:__
```
Input: [1,2,2,3]
Output: true
```
__Example 2:__
```
Input: [6,5,4,4]
Output: true
```
__Example 3:__
```
Input: [1,3,2]
Output: false
```
__Example 4:__
```
Input: [1,2,4,5]
Output: true
```
__Example 5:__
```
Input: [1,1,1]
Output: true
```

__Note:__
1. `1 <= A.length <= 50000`
2. `-100000 <= A[i] <= 100000`

### Solution
__O(n):__
```Swift
class Solution {
    func isMonotonic(_ A: [Int]) -> Bool {

        // Check if A is strictly increasing
        var isIncreasing = true
        for i in 0..<A.count-1 {
            if A[i] > A[i+1] {
                isIncreasing = false
                break
            }
        }
        if isIncreasing {
            return true
        }

        // Check if A is strictly decreasing
        isIncreasing = false
        for i in 0..<A.count-1 {
            if A[i] < A[i+1] {
                isIncreasing = true
                break
            }
        }
        return !isIncreasing
    }
}
```