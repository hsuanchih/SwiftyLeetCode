
### H-Index

Given an array of citations (each citation is a non-negative integer) of a researcher,</br> 
write a function to compute the researcher's h-index.

According to the definition of h-index on Wikipedia:</br> 
"A scientist has index *h* if *h* of his/her *N* papers have __at least__ h citations each,</br> 
and the other *N − h* papers have __no more than__ *h* citations each."

__Example:__
```
Input: citations = [3,0,6,1,5]
Output: 3 
Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had 
             received 3, 0, 6, 1, 5 citations respectively. 
             Since the researcher has 3 papers with at least 3 citations each and the remaining 
             two with no more than 3 citations each, her h-index is 3.
```

__Note:__ If there are several possible values for *h*, the maximum one is taken as the h-index.

### Solution
__O(n):__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        let citations = citations.sorted()
        for index in 0..<citations.count {
            let hIndex = citations.count-index
            if citations[index] >= hIndex {
                return hIndex
            }
        }
        return 0
    }
}
```