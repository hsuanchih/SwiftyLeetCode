
### Merge Intervals

Given an array of `intervals` where `intervals[i]` = `[starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

__Example 1:__
```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```
__Example 2:__
```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

__Constraints:__
* `1 <= intervals.length <= pow(10, 4)`
* `intervals[i].length == 2`
* `0 <= starti <= endi <= pow(10, 4)`

### Solution
__O(intervals * log(intervals) + intervals) Time, O(intervals) Space:__
```Swift
class Solution {
    func merge(_ intervals: [[Int]]) -> [[Int]] {
        let sortedIntervals: [[Int]] = intervals.sorted { $0[0] < $1[0] }
        var merged: [[Int]] = []
        for interval in sortedIntervals {
            if let previous = merged.last, (previous[0] ... previous[1]).contains(interval[0]) {
                _ = merged.removeLast()
                merged.append([previous[0], max(previous[1], interval[1])])
            } else {
                merged.append(interval)
            }
        }
        return merged
    }
}
```
__O(intervals * log(intervals) + intervals) Time, O(intervals) Space - Range Alternative:__
```Swift
class Solution {
    func merge(_ intervals: [[Int]]) -> [[Int]] {
        let sortedIntervals: [ClosedRange<Int>] = intervals
            .map { $0[0] ... $0[1] }
            .sorted { $0.lowerBound < $1.lowerBound }
            
        var merged: [ClosedRange<Int>] = []
        for interval in sortedIntervals {
            if let previous = merged.last, previous.contains(interval.lowerBound) {
                _ = merged.removeLast()
                merged.append(previous.lowerBound ... max(previous.upperBound, interval.upperBound))
            } else {
                merged.append(interval)
            }
        }
        return merged.map { [$0.lowerBound, $0.upperBound] }
    }
}
```