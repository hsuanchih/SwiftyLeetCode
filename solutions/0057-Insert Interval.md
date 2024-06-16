
### Insert Interval

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by starti. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` after the insertion.

Note that you don't need to modify `intervals` in-place. You can make a new array and return it.

__Example 1:__
```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```
__Example 2:__
```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

__Constraints:__
* `0 <= intervals.length <= pow(10, 4)`
* `intervals[i].length == 2`
* `0 <= starti <= endi <= pow(10, 5)`
* `intervals` is sorted by `starti` in __ascending order__.
* `newInterval.length == 2`
* `0 <= start <= end <= pow(10, 5)`

### Solution
__O(intervals) Time:__
```Swift
class Solution {
    func insert(_ intervals: [[Int]], _ newInterval: [Int]) -> [[Int]] {
        guard !intervals.isEmpty else { return [newInterval] }
        var merged: [[Int]] = []
        for interval in intervals {
            if let previous = merged.last {
                _ = merged.removeLast()
                merge(previous, interval).forEach { merged.append($0) }
            } else {
                merge(interval, newInterval).forEach { merged.append($0) }
            }
        }
        return merged
    }

    func merge(_ lhs: [Int], _ rhs: [Int]) -> [[Int]] {
        if lhs[0] < rhs[0] {
            return (lhs[0] ... lhs[1]).contains(rhs[0]) ? [[lhs[0], max(lhs[1], rhs[1])]] : [lhs, rhs]
        } else {
            return (rhs[0] ... rhs[1]).contains(lhs[0]) ? [[rhs[0], max(lhs[1], rhs[1])]] : [rhs, lhs]
        }
    }
}
```