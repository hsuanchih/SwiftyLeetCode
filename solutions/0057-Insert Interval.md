
### Insert Interval

Given a set of *non-overlapping* intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were __initially sorted__ according to their start times.

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

### Solution
__O(n):__
```Swift
class Solution {
    func insert(_ intervals: [[Int]], _ newInterval: [Int]) -> [[Int]] {
        var newInterval = newInterval, result : [[Int]] = []
        var index = 0
        while index < intervals.count {
            let interval = intervals[index],
            nextInsert = interval[0] < newInterval[0] ? interval : newInterval
            insert(nextInsert, into: &result)
            if nextInsert == interval {
                index+=1
            } else {
                newInterval = [Int.max, Int.max]
            }
        }
        if newInterval != [Int.max, Int.max] {
            insert(newInterval, into: &result)
        }
        return result
    }
    
    func insert(_ interval: [Int], into result: inout [[Int]]) {
        if let last = result.last, last[1] >= interval[0] {
            var elem = result.removeLast()
            elem[1] = max(elem[1], interval[1])
            result.append(elem)
        } else {
            result.append(interval)
        }
    }
}
```