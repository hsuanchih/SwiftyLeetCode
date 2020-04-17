
### Relative Sort Array

Given two arrays `arr1` and `arr2`, the elements of `arr2` are distinct, and all elements in `arr2` are also in `arr1`.

Sort the elements of `arr1` such that the relative ordering of items in `arr1` are the same as in `arr2`.</br>
Elements that don't appear in `arr2` should be placed at the end of `arr1` in __ascending__ order.


__Example 1:__
```
Input: arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
Output: [2,2,2,1,4,3,3,9,6,7,19]
```

__Constraints:__
* `arr1.length, arr2.length <= 1000`
* `0 <= arr1[i], arr2[i] <= 1000`
* Each `arr2[i]` is distinct.
* Each `arr2[i]` is in `arr1`.

### Solution
__O(arr1+arr2+1001) Time - Counting Sort:__
```Swift
class Solution {
    func relativeSortArray(_ arr1: [Int], _ arr2: [Int]) -> [Int] {
        var lookup : [Int] = Array(repeating: 0, count: 1001), 
        result : [Int] = Array(repeating: 0, count: arr1.count)
        arr1.forEach { lookup[$0]+=1 }
        var index = 0
        arr2.forEach {
            for _ in 0..<lookup[$0] {
                result[index] = $0
                index+=1
            }
            lookup[$0] = 0
        }
        for k in 0..<lookup.count {
            for _ in 0..<lookup[k] {
                result[index] = k
                index+=1
            }
        }
        return result
    }
}
```