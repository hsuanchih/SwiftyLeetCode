
### Search a 2D Matrix

You are given an `m x n` integer matrix `matrix` with the following two properties:
* Each row is sorted in non-decreasing order.
* The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` if target is in `matrix` or `false` otherwise.

You must write a solution in `O(log(m * n))` time complexity.

__Example 1:__

![question_74-0.jpg](../images/question_74-0.jpg)
```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```
__Example 2:__

![question_74-1.jpg](../images/question_74-1.jpg)
```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

__Constraints:__
* `m == matrix.length`
* `n == matrix[i].length`
* `1 <= m, n <= 100`
* `-pow(10, 4) <= matrix[i][j], target <= pow(10, 4)`

### Solution
__O(matrix) Time - Brute-Force:__
```Swift
class Solution{
    func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {
        for row in 0 ..< matrix.count {
            for col in 0 ..< matrix.first!.count where matrix[row][col] == target {
                return true
            }
        }
        return false
    }
}
```
__O(log(matrix)) Time - Binary-Search:__
```Swift
class Solution {
    func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {
        let numCols: Int = matrix.first!.count
        var start: Int = 0, end: Int = matrix.count * numCols - 1
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            switch matrix[mid / numCols][mid % numCols] {
            case target:
                return true
            case (target + 1)...:
                end = mid 
            case ..<target:
                start = mid
            default:
                fatalError()
            }
        }
        return matrix[start / numCols][start % numCols] == target || matrix[end / numCols][end % numCols] == target
    }
}
```