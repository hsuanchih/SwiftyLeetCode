
### H-Index

Given an array of integers `citations` where `citations[i]` is the number of citations a researcher received for their `ith` paper, return the researcher's h-index.

According to the definition of h-index on Wikipedia: The h-index is defined as the maximum value of `h` such that the given researcher has published at least `h` papers that have each been cited at least `h` times.

__Example 1:__
```
Input: citations = [3,0,6,1,5]
Output: 3
Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively.
Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3.
```
__Example 2:__
```
Input: citations = [1,3,1]
Output: 1
```

__Constraints:__
* `n == citations.length`
* `1 <= n <= 5000`
* `0 <= citations[i] <= 1000`

### Solution
__O(pow(citations, 2)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        guard citations.count > 0 else { return 0 }
        var result: Int = 0
        for papers in 1 ... citations.count {
            var numCitationsGreaterThanOrEqualToPapers: Int = 0
            for citation in citations where citation >= papers {
                numCitationsGreaterThanOrEqualToPapers += 1
                if numCitationsGreaterThanOrEqualToPapers == papers {
                    result = papers
                    break
                }
            }
        }
        return result
    }
}
```
__O(pow(citations, 2)) Time, O(1) Space - Brute-Force Optimized:__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        for papers in stride(from: citations.count, through: 1, by: -1) {
            var numCitationsGreaterThanOrEqualToPapers: Int = 0
            for citation in citations where citation >= papers {
                numCitationsGreaterThanOrEqualToPapers += 1
                if numCitationsGreaterThanOrEqualToPapers == papers {
                    return papers
                }
            }
        }
        return 0
    }
}
```
__O(citations * log(citations) + 2 * citations) Time, O(1) Space - Sorted Input:__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        let citations: [Int] = citations.sorted().reversed()
        for i in 0 ..< citations.count where i >= citations[i] {
            return i
        }
        return citations.count
    }
}
```
__O(citations * log(citations) + citations) Time, O(1) Space - Sorted Input:__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        let citations = citations.sorted()
        var result: Int = 0
        for i in stride(from: citations.count - 1, through: 0, by: -1) where citations[i] >= citations.count - i {
            result = max(result, citations.count - i)
        }
        return result
    }
}
```
__O(2 * citations) Time, O(citations) Space - Bucket Sort:__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        var numPapersByCitation: [Int] = Array(repeating: 0, count: citations.count + 1)
        for citation in citations {
            numPapersByCitation[min(citation, citations.count)] += 1
        }

        var numCitations: Int = 0
        for h in stride(from: citations.count, through: 0, by: -1) {
            numCitations += numPapersByCitation[h]
            if h <= numCitations {
                return h
            }
        }
        return 0
    }
}
```