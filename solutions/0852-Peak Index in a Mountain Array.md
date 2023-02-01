
### Peak Index in a Mountain Array

An array `arr` a __mountain__ if the following properties hold:

* `arr.length >= 3`
* There exists some `i` with `0 < i < arr.length - 1` such that:
   * `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
   * `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given a mountain array `arr`, return the index `i` such that `arr[0] < arr[1] < ... < arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`.

You must solve it in `O(log(arr.length))` time complexity.


__Example 1:__
```
Input: arr = [0,1,0]
Output: 1
```
__Example 2:__
```
Input: arr = [0,2,1,0]
Output: 1
```
__Example 3:__
```
Input: arr = [0,10,5,2]
Output: 1
```

__Constraints:__
* `3 <= arr.length <= 10^5`
* `0 <= arr[i] <= 10^6`
* `arr` is __guaranteed__ to be a mountain array.

### Solution
__O(arr) Time - Brute-Force:__
```Swift
class Solution {
    func peakIndexInMountainArray(_ arr: [Int]) -> Int {
        for i in stride(from: 1, to: arr.count - 1, by: 1) {
            if arr[i - 1] < arr[i] && arr[i] > arr[i + 1] {
                return i
            }
        }
        return arr.count - 1
    }
}
```
__O(log\[base 2\](arr)) Time - Binary-Search:__
```Swift
class Solution {
    func peakIndexInMountainArray(_ arr: [Int]) -> Int {
        var start: Int = 0, end: Int = arr.count - 1
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            if mid == 0 || mid == arr.count - 1 {
                return mid
            }
            switch (arr[mid - 1], arr[mid + 1]) {
            case (Int.min ..< arr[mid], arr[mid] + 1 ... Int.max):
                start = mid
            case (arr[mid] + 1 ... Int.max, Int.min ..< arr[mid]):
                end = mid
            case (Int.min ..< arr[mid], Int.min ..< arr[mid]):
                return mid
            default:
                fatalError()
            }
        }
        return arr[start] > arr[end] ? start : end
    }
}
```