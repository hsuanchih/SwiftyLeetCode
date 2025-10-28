
### H-Index II

Given an array of integers `citations` where `citations[i]` is the number of citations a researcher received for their `ith` paper and `citations` is __sorted in ascending order__, return the researcher's h-index.

According to the definition of h-index on Wikipedia: The h-index is defined as the maximum value of `h` such that the given researcher has published at least `h` papers that have each been cited at least `h` times.

You must write an algorithm that runs in logarithmic time.

__Example 1:__
```
Input: citations = [0,1,3,5,6]
Output: 3
Explanation: [0,1,3,5,6] means the researcher has 5 papers in total and each of them had received 0, 1, 3, 5, 6 citations respectively.
Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3.
```
__Example 2:__
```
Input: citations = [1,2,100]
Output: 2
```

__Constraints:__
* `n == citations.length`
* `1 <= n <= pow(10, 5)`
* `0 <= citations[i] <= 1000`
* citations is __sorted in ascending order__.

### Solution
__O(citations) Time, Linear Search:__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        for i in 0 ..< citations.count where citations[i] >= citations.count - i {
            return citations.count - i
        }
        return 0
    }
}
```
__O(log(citations)) Time, Binary Search:__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        // Binary search elements in citations to find the maximum H-Index
        var start: Int = 0, end: Int = citations.count - 1
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            // If all citations.count - mid publications have more than citations[mid] citations,
            // we've found a valid H-Index, but we want the maximum valid H-Index, so continue search to the left
            if citations[mid] >= citations.count - mid {
                end = mid
            } else {
                start = mid
            }
        }

        // Prioritize the larger H-Index over the smaller H-Index:
        // check start before end
        if citations[start] >= citations.count - start {
            return citations.count - start
        } else if citations[end] >= citations.count - end {
            return citations.count - end
        } else {
            return 0
        }
    }
}
```