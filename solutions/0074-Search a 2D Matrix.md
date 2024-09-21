
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
__O(log\[base 2\](matrix)) Time - Binary-Search:__
```Swift
class Solution {
    func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {
        let rows = matrix.count, cols = matrix.first?.count ?? 0
        if rows == 0 || cols == 0 {
            return false
        }
        var start = 0, end = rows*cols-1
        while start+1 < end {
            let mid = start + (end-start)/2
            switch matrix[mid/cols][mid%cols] {
                case target:
                return true
                case Int.min..<target:
                start = mid
                default:
                end = mid
            }
        }
        if matrix[start/cols][start%cols] == target || matrix[end/cols][end%cols] == target {
            return true
        }
        return false
    }
}
```