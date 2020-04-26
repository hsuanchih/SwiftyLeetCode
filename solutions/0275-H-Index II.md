
### H-Index II

Given an array of citations __sorted in ascending order__ (each citation is a non-negative integer) of a researcher,</br> 
write a function to compute the researcher's h-index.

According to the definition of h-index on Wikipedia:</br> 
"A scientist has index *h* if *h* of his/her *N* papers have __at least__ *h* citations each,</br> 
and the other *N − h* papers have __no more than__ *h* citations each."

__Example:__
```
Input: citations = [0,1,3,5,6]
Output: 3 
Explanation: [0,1,3,5,6] means the researcher has 5 papers in total and each of them had 
             received 0, 1, 3, 5, 6 citations respectively. 
             Since the researcher has 3 papers with at least 3 citations each and the remaining 
             two with no more than 3 citations each, her h-index is 3.
```

__Note:__
If there are several possible values for *h*, the maximum one is taken as the h-index.

__Follow up:__
* This is a follow up problem to `H-Index`, where `citations` is now guaranteed to be sorted in ascending order.
* Could you solve it in logarithmic time complexity?

### Solution
__O(log\[base 2\](citations)):__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        if citations.isEmpty { return 0 }

        // Binary search elements in citations to find the maximum H-Index
        var start = 0, end = citations.count-1
        while start+1 < end {
            let mid = start + (end-start)/2, hIndex = citations.count-mid

            // If all (citations.count-mid) publications have more than (citations.count-mid) citations,
            // we've found a valid H-Index (citations.count-mid)
            // But we want the maximum valid H-Index, so continue search to the left
            if citations[mid] >= hIndex {
                end = mid
            } else {
                start = mid
            }
        }

        // Prioritize the larger H-Index over the smaller H-Index:
        // check start before end
        switch (citations[start], citations[end]) {
            case (citations.count-start...Int.max, _):
            return citations.count-start
            case (_, citations.count-end...Int.max):
            return citations.count-end
            default:
            return 0
        }
    }
}
```