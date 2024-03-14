
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
__O(intervals * log(intervals) + intervals) Time, O(intervals) Space:__
```Swift
class Solution {
    func merge(_ intervals: [[Int]]) -> [[Int]] {
        guard !intervals.isEmpty else { return [] }
        let intervals: [[Int]] = intervals.sorted { $0.first! < $1.first! }
        var result: [[Int]] = []
        for interval in intervals {
            if let previous = result.last, (previous.first! ... previous.last!).contains(interval.first!) {
                result.removeLast()
                result.append([previous.first!, max(previous.last!, interval.last!)])
            } else {
                result.append(interval)
            }
        }
        return result
    }
}
```