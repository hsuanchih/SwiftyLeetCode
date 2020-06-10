
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
__O(n^2) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        for index in stride(from: citations.count-1, through: 0, by: -1) {
            var count = 0
            for citation in citations {
                if citation >= index+1 {
                    count+=1
                }
            }
            if count >= index+1 {
                return index+1
            }
        }
        return 0
    }
}
```
__O(n) Time, O(n) Space - Bucket Sort:__
```Swift
class Solution {
    func hIndex(_ citations: [Int]) -> Int {
        var count : [Int] = Array(repeating: 0, count: citations.count+1)
        for citation in citations {
            if citation >= citations.count {
                count[citations.count]+=1
            } else {
                count[citation]+=min(1, citation)
            }
        }
        var citationCount : Int = 0
        for index in stride(from: citations.count, through: 0, by: -1) {
            citationCount+=count[index]
            if citationCount >= index {
                return index
            }
        }
        return 0
    }
}
```