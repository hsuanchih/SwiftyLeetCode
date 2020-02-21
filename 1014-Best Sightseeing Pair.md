
### Best Sightseeing Pair

Given an array `A` of positive integers, `A[i]` represents the value of the `i`-th sightseeing spot, and two sightseeing spots `i` and `j` have distance `j - i` between them.

The score of a pair (`i < j`) of sightseeing spots is (`A[i] + A[j] + i - j`) : the sum of the values of the sightseeing spots, __minus__ the distance between them.

Return the maximum score of a pair of sightseeing spots.

__Example 1:__
```
Input: [8,1,5,2,6]
Output: 11
Explanation: i = 0, j = 2, A[i] + A[j] + i - j = 8 + 5 + 0 - 2 = 11
```

__Note:__
1. `2 <= A.length <= 50000`
2. `1 <= A[i] <= 1000`

### Solution
__O(n^2):__
```Swift
class Solution {
    /// For every i, iterate through all j > i to compute 
    /// A[i] + A[j] + i - j
    /// and track the maximum score
    func maxScoreSightseeingPair(_ A: [Int]) -> Int {
        var result = 0
        for i in 0..<A.count-1 {
            for j in i+1..<A.count {
                result = max(result, A[i]+A[j]+i-j)
            }
        }
        return result
    }
}
```
__O(n):__
```Swift
class Solution {
    /// Since j is guaranteed to be always greater than i,
    /// the array can be iterated over once while keeping track of the maximum A[i]+i (maxSoFar)
    /// and computing maxSoFar+A[i]-i for every index that follow (A[i]+A[j]+i-j = (A[i]+i)+(A[j]-j)), 
    /// while keeping track of the maximum score
    func maxScoreSightseeingPair(_ A: [Int]) -> Int {
        var maxSoFar = A.first!, result = 0
        for i in 1..<A.count {
            result = max(result, maxSoFar+A[i]-i)
            maxSoFar = max(maxSoFar, A[i]+i)
        }
        return result
    }
}
```