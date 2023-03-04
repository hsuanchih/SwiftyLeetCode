
### Flood Fill

An image is represented by an `m x n` integer grid `image` where `image[i][j]` represents the pixel value of the image.

You are also given three integers `sr`, `sc`, and `newColor`. You should perform a flood fill on the image starting from the pixel `image[sr][sc]`.

To perform a __flood fill__, consider the starting pixel, plus any pixels connected __4-directionally__ to the starting pixel of the same color as the starting pixel, plus any pixels connected __4-directionally__ to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with `newColor`.

Return _the modified image after performing the flood fill_.

__Example 1:__

![images/question_733.jpg](images/question_733.jpg)

```
Input: 
image = [
    [1,1,1],
    [1,1,0],
    [1,0,1]
], 
sr = 1, sc = 1, newColor = 2

Output: [[2,2,2],[2,2,0],[2,0,1]]

Explanation: From the center of the image with position (sr, sc) = (1, 1) (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color.
Note the bottom corner is not colored 2, because it is not 4-directionally connected to the starting pixel.
```

__Example 2:__
```
Input: 
image = [
    [0,0,0],
    [0,0,0]
], 
sr = 0, sc = 0, newColor = 2

Output: [[2,2,2],[2,2,2]]
```

__Constraints:__
* `m == image.length`
* `n == image[i].length`
* `1 <= m, n <= 50`
* `0 <= image[i][j], newColor < 216`
* `0 <= sr < m`
* `0 <= sc < n`

### Solution
__O(2\*(m*n)) Time:__
```Swift
class Solution {
    func floodFill(_ image: [[Int]], _ sr: Int, _ sc: Int, _ newColor: Int) -> [[Int]] {
        guard !image.isEmpty else { return image }
        var image = image, color = image[sr][sc]
        fill(image: &image, row: sr, col: sc, color: color)

        // Traverse image & replace -1's with newColor
        (0 ..< image.count).forEach { row in
            (0 ..< image.first!.count).forEach { col in
                if image[row][col] == -1 {
                    image[row][col] = newColor
                }
            }
        }
        return image
    }
    
    /// If the pixel in question is the same color as image[sr][sc], we
    /// 1. Assign the pixel a placeholder color of -1
    /// 2. Visit the pixel's neighboring pixel & repeat steps 1 & 2
    func fill(image: inout [[Int]], row: Int, col: Int, color: Int) {
        switch (row, col) {
        case (0 ..< image.count, 0 ..< image.first!.count) where image[row][col] == color:
            // Placeholder color -1 is assigned here intead of newColor to prevent
            // non-terminating recursion in the event that image[sr][sc] is equal to newColor.
            image[row][col] = -1
            [-1, 1].forEach {
                fill(image: &image, row: row+$0, col: col, color: color)
                fill(image: &image, row: row, col: col+$0, color: color)
            }
        default:
            break
        }
    }
}
```

__O(m*n) Time:__
```Swift
class Solution {
    func floodFill(_ image: [[Int]], _ sr: Int, _ sc: Int, _ newColor: Int) -> [[Int]] {
        // If color is the same as newColor, the original image 
        // would be equivalent to the transformed image
        guard sr >= 0,  sr < image.count,  sc >= 0,  sc < (image.first?.count ?? 0), image[sr][sc] != color else {
            return image
        }
        var image: [[Int]] = image
        replace(image: &image, row: sr, col: sc, color: image[sr][sc], newColor: color)
        return image
    }

    func replace(image: inout [[Int]], row: Int, col: Int, color: Int, newColor: Int) {
        guard row >= 0,  row < image.count,  col >= 0,  col < (image.first?.count ?? 0), image[row][col] == color else { return }
        image[row][col] = newColor
        [-1, 1].forEach {
            replace(image: &image, row: row + $0, col: col, color: color, newColor: newColor)
            replace(image: &image, row: row, col: col + $0, color: color, newColor: newColor)
        }
    }
}
```