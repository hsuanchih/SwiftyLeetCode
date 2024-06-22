
### Minimum Number of Arrows to Burst Balloons

There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where `points[i] = [xstart, xend]` denotes a balloon whose horizontal diameter stretches between `xstart` and `xend`. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up __directly vertically__ (in the positive y-direction) from different points along the x-axis. A balloon with `xstart` and `xend` is burst by an arrow shot at `x` if `xstart <= x <= xend`. There is no limit to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return the __minimum number of arrows__ that must be shot to burst all balloons.

__Example 1:__
```
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].
- Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].
```
__Example 2:__
```
Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4
Explanation: One arrow needs to be shot for each balloon for a total of 4 arrows.
```
__Example 3:__
```
Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 2, bursting the balloons [1,2] and [2,3].
- Shoot an arrow at x = 4, bursting the balloons [3,4] and [4,5].
```
 
__Constraints:__
* `1 <= points.length <= pow(10, 5)`
* `points[i].length == 2`
* `-pow(2, 31) <= xstart < xend <= pow(2, 31) - 1`

### Solution
__O(points + points * log(points)) Time, O(points) Space:__
```Swift
class Solution {
    func findMinArrowShots(_ points: [[Int]]) -> Int {
        let points: [[Int]] = points.sorted { $0[0] < $1[0] }
        var intersects: [[Int]] = []
        for point in points {
            if let previous = intersects.last, (previous[0] ... previous[1]).contains(point[0]) {
                _ = intersects.removeLast()
                intersects.append([previous[0], min(previous[1], point[1])])
            } else {
                intersects.append(point)
            }
        }
        return intersects.count
    }
}
```