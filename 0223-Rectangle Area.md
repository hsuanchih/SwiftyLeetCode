
### Rectangle Area

Find the total area covered by two __rectilinear__ rectangles in a __2D__ plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

Rectangle Area

__Example:__
```
Input: A = -3, B = 0, C = 3, D = 4, E = 0, F = -1, G = 9, H = 2
Output: 45
```

__Note:__
Assume that the total area is never beyond the maximum possible value of int.

### Solution
```Swift
class Solution {
    
    // Data structure representing point
    private struct Point {
        var x: Int, y: Int
    }
    
    func computeArea(_ A: Int, _ B: Int, _ C: Int, _ D: Int, _ E: Int, _ F: Int, _ G: Int, _ H: Int) -> Int {
        let rect1 = area(Point(x:A,y:B), Point(x:C,y:D)), rect2 = area(Point(x:E,y:F), Point(x:G,y:H))
        let x1 = max(A,E), x2 = min(C,G), y1 = max(B,F), y2 = min(D,H)
        if x1 >= x2 || y1 >= y2 {
            return rect1+rect2
        }
        return rect1+rect2-area(Point(x:x1, y:y1), Point(x:x2, y:y2))
    }
    
    // Method to compute the area given points p1 & p2
    private func area(_ p1: Point, _ p2: Point) -> Int {
        return abs(p2.x-p1.x)*abs(p2.y-p1.y)
    }
}
```