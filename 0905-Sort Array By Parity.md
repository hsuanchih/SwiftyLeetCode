
### Sort Array By Parity

Given an array `A` of non-negative integers, return an array consisting of all the even elements of `A`, followed by all the odd elements of `A`.

You may return any answer array that satisfies this condition.

__Example 1:__
```
Input: [3,1,2,4]
Output: [2,4,3,1]
The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.
```

__Note:__
1. `1 <= A.length <= 5000`
2. `0 <= A[i] <= 5000`

### Solution
__O(n) Time, O(n) Space:__
```Swift
class Solution {
    func sortArrayByParity(_ A: [Int]) -> [Int] {
        var result = A, start = 0, end = result.count-1
        for num in A {
            if num%2 == 0 {
                result[start] = num
                start+=1
            } else {
                result[end] = num
                end-=1
            }
        }
        return result
    }
}
```
__O(n) Time, In-place:__
```Swift
class Solution {
    func sortArrayByParity(_ A: [Int]) -> [Int] {

        // Elements to the left of i are even numbers
        var A = A, i = 0
        for j in 0..<A.count {
            if A[j]%2 == 0 {
                if i != j {
                    A.swapAt(i, j)
                }
                i+=1
            }
        }
        return A
    }
}
```