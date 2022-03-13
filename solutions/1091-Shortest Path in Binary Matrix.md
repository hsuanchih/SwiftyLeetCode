
### Shortest Path in Binary Matrix

Given an `n x n` binary matrix `grid`, return _the length of the shortest **clear path** in the matrix_. If there is no clear path, return `-1`.

A __clear path__ in a binary matrix is a path from the __top-left__ cell (i.e., `(0, 0)`) to the __bottom-right__ cell (i.e., `(n - 1, n - 1)`) such that:

* All the visited cells of the path are `0`.
* All the adjacent cells of the path are __8-directionally__ connected (i.e., they are different and they share an edge or a corner).

The __length of a clear path__ is the number of visited cells of this path.

__Example 1:__

![images/question_1091-1.png](images/question_1091-1.png)

```
Input: grid = [
    [0,1],
    [1,0]
]

Output: 2
```

__Example 2:__

![images/question_1091-2.png](images/question_1091-2.png)

```
Input: grid = [
    [0,0,0],
    [1,1,0],
    [1,1,0]
]

Output: 4
```

__Example 3:__

```
Input: grid = [
    [1,0,0],
    [1,1,0],
    [1,1,0]
]

Output: -1
```

__Constraints:__
* `n == grid.length`
* `n == grid[i].length`
* `1 <= n <= 100`
* `grid[i][j] is 0 or 1`

### Solution
__O(n*n), BFS:__
```swift
class Solution {
    struct Point: Hashable {
        let x: Int, y: Int
    }
    
    func shortestPathBinaryMatrix(_ grid: [[Int]]) -> Int {
        guard !grid.isEmpty, let x = grid.first, !x.isEmpty else { return -1 }
        let start = Point(x: 0, y: 0), end = Point(x: x.count - 1, y: grid.count - 1)
        if grid[start.y][start.x] == 1 || grid[end.y][end.x] == 1 {
            return -1
        }
        var queue: [Point] = [Point(x: 0, y: 0)], visited: Set<Point> = Set(queue)
        var pathLength = 0
        while !queue.isEmpty {
            let count = queue.count
            for _ in 0 ..< count {
                let current = queue.removeFirst()
                if current == end {
                    return pathLength + 1
                }
                [-1, 1].forEach {
                    [Point(x: current.x + $0, y: current.y), Point(x: current.x, y: current.y + $0), Point(x: current.x + $0, y: current.y + $0), Point(x: current.x - $0, y: current.y + $0)].forEach { point in
                        switch (point.x, point.y) {
                        case (0 ... end.x, 0 ... end.y) where grid[point.y][point.x] == 0 && !visited.contains(point):
                            queue.append(point)
                            visited.insert(point)
                        default:
                            break
                        }
                    }
                }
                
            }
            pathLength += 1
        }
        return -1
    }
}
```