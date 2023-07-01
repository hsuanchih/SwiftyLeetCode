
### Kth Missing Positive Number

Given an array `arr` of positive integers sorted in a __strictly increasing order__, and an integer `k`.

Return the `kth` _positive integer that is missing from this array_.

__Example 1:__
```
Input: arr = [2,3,4,7,11], k = 5
Output: 9
Explanation: The missing positive integers are [1,5,6,8,9,10,12,13,...]. The 5th missing positive integer is 9.
```
__Example 2:__
```
Input: arr = [1,2,3,4], k = 2
Output: 6
Explanation: The missing positive integers are [5,6,7,...]. The 2nd missing positive integer is 6.
```

__Constraints:__
* `1 <= arr.length <= 1000`
* `1 <= arr[i] <= 1000`
* `1 <= k <= 1000`
* `arr[i] < arr[j]` for `1 <= i < j <= arr.length`

### Solution
__O(arr) Time, O(arr) Space - Linear Search:__
```Swift
class Solution {
    func findKthPositive(_ arr: [Int], _ k: Int) -> Int {
        guard !arr.isEmpty, k >= 1 else { fatalError() }
        var k: Int = k
        let nums: Set<Int> = Set(arr)
        for i in 1 ... arr.last! {
            // If i is not in arr, decrement k
            if !nums.contains(i) {
                k -= 1
            }
            // If k is 0, we've found the k-th number missing from arr 
            if k == 0 {
                return i
            }
        }
        // If we've reached the end of the loop without k reaching 0,
        // then the k-th number must be the last number in arr plus what remains of k.
        return arr.last! + k
    }
}
```
__O(arr) Time, O(1) Sapce - Linear Tally:__
```Swift
class Solution {
    func findKthPositive(_ arr: [Int], _ k: Int) -> Int {
        guard !arr.isEmpty, k >= 1 else { fatalError() }
        var k: Int = k
        for i in 0 ... arr.count - 1 {
            let previousValue: Int = i == 0 ? 0 : arr[i - 1]
            // numsSkipped is greater than 0 only if number at the current index
            // exceeds the number at the previous index by more than 1
            let numsSkipped: Int = arr[i] - previousValue - 1

            // If the numbers skipped at the current index exceeds k,
            // then we know that the previous value + k is the k-th missing number.
            // Otherwise move to the next number
            if numsSkipped >= k {
                return previousValue + min(numsSkipped, k)
            } else {
                k -= numsSkipped
            }
        }
        // If we've reached the end of the loop without k reaching 0,
        // then the k-th number must be the last number in arr plus what remains of k.
        return arr.last! + k
    }
}
```
__O(log\[base 2\](arr)) Time, O(1) Space - Binary Search:__
```Swift
class Solution {
    func findKthPositive(_ arr: [Int], _ k: Int) -> Int {
        guard !arr.isEmpty, k >= 1 else { fatalError() }
        var start: Int = 0, end: Int = arr.count - 1
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            // The numbers skipped at a specific index is the number at the index - (index + 1).
            // Use binary search to find the first index at which the numbers skipped is larger than k.
            let numsSkippedAtMid = arr[mid] - (mid + 1)
            if numsSkippedAtMid >= k {
                end = mid
            } else {
                start = mid
            }
        }

        if arr[start] - (start + 1) >= k {
            return k
        } else if arr[end] - (end + 1) < k {
            return end + 1 + k
        } else {
            return start + 1 + k
        }
    }
}
```