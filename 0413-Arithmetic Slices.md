
### Arithmetic Slices

A sequence of number is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

For example, these are arithmetic sequence:
```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```
The following sequence is not arithmetic.
```
1, 1, 2, 5, 7
```

A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.

A slice (P, Q) of array A is called arithmetic if the sequence:
A[P], A[p + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.

The function should return the number of arithmetic slices in the array A.


__Example:__
```
A = [1, 2, 3, 4]

return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.
```

### Solution
__O(A^2) Time, O(1) Space - Brute Force:__
```Swift
class Solution {
    func numberOfArithmeticSlices(_ A: [Int]) -> Int {
        guard A.count >= 3 else { return 0 } 
        var result = 0
        for i in 0..<A.count-2 {
            var difference = A[i+1] - A[i]
            for j in i+1..<A.count-1 {
                if A[j+1] - A[j] != difference {
                    break
                }
                result+=1
            }
        }
        return result
    }
}
```
__O(A) Time, O(A) Space - Memoization:__
```Swift
class Solution {
    func numberOfArithmeticSlices(_ A: [Int]) -> Int {
        guard A.count >= 3 else { return 0 }
        var memo : [Int] = Array(repeating: 0, count: A.count)
        var result = 0
        for i in 2..<A.count {
            if A[i]-A[i-1] == A[i-1]-A[i-2] {
                memo[i] = memo[i-1]+1
                result += memo[i]
            } 
        }
        return result
    }
}
```