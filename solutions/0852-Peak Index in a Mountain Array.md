
### Peak Index in a Mountain Array

Let's call an array `A` a mountain if the following properties hold:

* `A.length >= 3`
* There exists some `0 < i < A.length - 1` such that `A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]`

Given an array that is definitely a mountain, return any `i` such that `A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]`.

__Example 1:__
```
Input: [0,1,0]
Output: 1
```
__Example 2:__
```
Input: [0,2,1,0]
Output: 1
```

__Note:__
1. `3 <= A.length <= 10000`
2. `0 <= A[i] <= 10^6`
3. `A` is a mountain, as defined above.

### Solution
__O(A) Time - Brute-Force:__
```Swift
class Solution {
    func peakIndexInMountainArray(_ A: [Int]) -> Int {
        for i in 0..<A.count {
            if i != 0 && i != A.count-1 && A[i] > A[i-1] && A[i] > A[i+1] {
                return i
            }
        }
        fatalError()
    }
}
```
__O(log\[base 2\](A)) Time - Binary-Search:__
```Swift
class Solution {
    func peakIndexInMountainArray(_ A: [Int]) -> Int {
        var start = 0, end = A.count-1
        while start+1 < end {
            let mid = start + (end-start)/2
            if A[mid] > A[mid-1] {
                if A[mid] > A[mid+1] {
                    return mid
                }
                start = mid
            } else {
                end = mid
            }
        }
        return A[start] > A[end] ? start : end
    }
}
```