
### N-Repeated Element in Size 2N Array

In a array `A` of size `2N`, there are `N+1` unique elements, and exactly one of these elements is repeated N times.

Return the element repeated N times.

__Example 1:__
```
Input: [1,2,3,3]
Output: 3
```
__Example 2:__
```
Input: [2,1,2,5,3,2]
Output: 2
```
__Example 3:__
```
Input: [5,1,5,2,5,3,5,4]
Output: 5
```

### Solution
__O(n):__
```Swift
class Solution {
    func repeatedNTimes(_ A: [Int]) -> Int {
        var seen : Set<Int> = []
        for elem in A {
            if seen.contains(elem) {
                return elem
            }
            seen.insert(elem)
        }
        fatalError()
    }
}
```