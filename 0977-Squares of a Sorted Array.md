
### Squares of a Sorted Array

Given an array of integers `A` sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

__Example 1:__
```
Input: [-4,-1,0,3,10]
Output: [0,1,9,16,100]
```
__Example 2:__
```
Input: [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

__Note:__
1. `1 <= A.length <= 10000`
2. `-10000 <= A[i] <= 10000`
3. `A` is sorted in non-decreasing order.

### Solution
__O(n*log(n)+n):__
```Swift
class Solution {
    func sortedSquares(_ A: [Int]) -> [Int] {
        return A.map { $0*$0 }.sorted()
    }
}
```
__O(n):__
```Swift
class Solution {
    func sortedSquares(_ A: [Int]) -> [Int] {
        var result = A, start = 0, end = A.count-1, i = A.count-1
        while start < end {
            let left = abs(A[start]), right = abs(A[end])
            if left < right {
                result[i] = right*right
                end-=1
            } else {
                result[i] = left*left
                start+=1
            }
            i-=1
        }
        result[i] = A[start]*A[start]
        return result
    }
}
```