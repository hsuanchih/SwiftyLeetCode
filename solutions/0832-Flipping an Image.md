
### Flipping an Image

Given a binary matrix `A`, we want to flip the image horizontally, then invert it, and return the resulting image.

To flip an image horizontally means that each row of the image is reversed.</br>
For example, flipping `[1, 1, 0]` horizontally results in `[0, 1, 1]`.

To invert an image means that each `0` is replaced by `1`, and each `1` is replaced by `0`.</br>
For example, inverting `[0, 1, 1]` results in `[1, 0, 0]`.

__Example 1:__
```
Input: [[1,1,0],[1,0,1],[0,0,0]]
Output: [[1,0,0],[0,1,0],[1,1,1]]
Explanation: First reverse each row: [[0,1,1],[1,0,1],[0,0,0]].
Then, invert the image: [[1,0,0],[0,1,0],[1,1,1]]
```
__Example 2:__
```
Input: [[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]
Output: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
Explanation: First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]].
Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
```

__Notes:__
* `1 <= A.length = A[0].length <= 20`
* `0 <= A[i][j] <= 1`

### Solution
__O(A) Time:__
```Swift
extension Int {
    mutating func invert() {
        self ^= 1
    }
}

class Solution {
    func flipAndInvertImage(_ A: [[Int]]) -> [[Int]] {
        var A = A
        for row in 0..<A.count {
            var start = 0, end = A.first!.count-1
            while start <= end {
                if start != end {
                    A[row].swapAt(start, end)
                    A[row][start].invert()
                }
                A[row][end].invert()
                start+=1
                end-=1
            }
        }
        return A
    }
}
```