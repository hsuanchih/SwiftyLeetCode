
### Find the Distance Value Between Two Arrays

Given two integer arrays `arr1` and `arr2`, and the integer `d`, return the distance value between the two arrays.

The distance value is defined as the number of elements `arr1[i]` such that there is not any element `arr2[j]` where `|arr1[i]-arr2[j]| <= d`.

__Example 1:__
```
Input: arr1 = [4,5,8], arr2 = [10,9,1,8], d = 2
Output: 2
Explanation: 
For arr1[0]=4 we have: 
|4-10|=6 > d=2 
|4-9|=5 > d=2 
|4-1|=3 > d=2 
|4-8|=4 > d=2 
For arr1[1]=5 we have: 
|5-10|=5 > d=2 
|5-9|=4 > d=2 
|5-1|=4 > d=2 
|5-8|=3 > d=2
For arr1[2]=8 we have:
|8-10|=2 <= d=2
|8-9|=1 <= d=2
|8-1|=7 > d=2
|8-8|=0 <= d=2
```
__Example 2:__
```
Input: arr1 = [1,4,2,3], arr2 = [-4,-3,6,10,20,30], d = 3
Output: 2
```
__Example 3:__
```
Input: arr1 = [2,1,100,3], arr2 = [-5,-2,10,-3,7], d = 6
Output: 1
```

__Constraints:__
* `1 <= arr1.length, arr2.length <= 500`
* `-1000 <= arr1[i], arr2[j] <= 1000`
* `0 <= d <= 100`

### Solution
__O(arr1 * arr2) Time - Exhaustive Search:__
```Swift
class Solution {
    func findTheDistanceValue(_ arr1: [Int], _ arr2: [Int], _ d: Int) -> Int {
        var count: Int = 0
        for i in 0 ..< arr1.count {
            for j in 0 ..< arr2.count {
                if abs(arr1[i] - arr2[j]) <= d {
                    break
                } else if j == arr2.count - 1 {
                    count += 1
                }
            }
        }
        return count
    }
}
```
__O((arr2 * log(arr2)) + (arr1 * log(arr2))) Time - Binary Search:__
```Swift
class Solution {
    func findTheDistanceValue(_ arr1: [Int], _ arr2: [Int], _ d: Int) -> Int {
        var count: Int = 0
        let arr2: [Int] = arr2.sorted()
        for i in 0 ..< arr1.count {
            var start: Int = 0, end: Int = arr2.count - 1
            while start + 1 < end {
                let mid: Int = start + (end - start) / 2
                switch arr2[mid] - arr1[i] {
                case 0:
                    start = mid
                    end = mid
                case ..<0:
                    start = mid
                case 1...:
                    end = mid
                default:
                    fatalError()
                }
            }
            if abs(arr1[i] - arr2[start]) > d, abs(arr1[i] - arr2[end]) > d {
                count += 1
            }
        }
        return count
    }
}
```