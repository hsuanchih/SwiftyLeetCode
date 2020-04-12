
### Merge Intervals

Given a collection of intervals, merge all overlapping intervals.

__Example 1:__
```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```
__Example 2:__
```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

### Solution
__O(intervals*log\[base 2\](intervals)+intervals) Time, O(intervals) Space:__
```Swift
class Solution {
    func merge(_ intervals: [[Int]]) -> [[Int]] {
        let input = intervals.sorted { $0[0] < $1[0] }
        var result : [[Int]] = []
        for interval in input {
            if let last = result.last, last[1] >= interval[0] {
                var temp = result.removeLast()
                temp[1] = max(temp[1], interval[1])
                result.append(temp)
            } else {
                result.append(interval)
            }
        }
        return result
    }
}
```