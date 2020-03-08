
### Contains Duplicate II

Given an array of integers and an integer *k*, find out whether there are two distinct indices *i* and *j*</br> 
in the array such that __nums[i] = nums[j]__ and the __absolute__ difference between *i* and *j* is at most *k*.

__Example 1:__
```
Input: nums = [1,2,3,1], k = 3
Output: true
```
__Example 2:__
```
Input: nums = [1,0,1,1], k = 1
Output: true
```
__Example 3:__
```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

### Solution
__O(n^2) Time, O(1) Space:__
```Swift
class Solution {
    func containsNearbyDuplicate(_ nums: [Int], _ k: Int) -> Bool {
        guard k > 0 else { return false }
        for i in 0..<nums.count {
            for j in i+1...i+k where j < nums.count {
                if nums[i] == nums[j] && j-i <= k {
                    return true
                }
            }
        }
        return false
    }
}
```
__O(n) Time, O(n) Space:__
```Swift
class Solution {
    func containsNearbyDuplicate(_ nums: [Int], _ k: Int) -> Bool {

        // lookup stores each unique element in nums & its most recent
        // index in nums
        var lookup : [Int: Int] = [:]

        for j in 0..<nums.count {
            let num = nums[j]

            // If previous index of the current element is less than k apart
            // from the current index, return true
            // Otherwise update lookup with the current index
            if let i = lookup[num], j-i <= k {
                return true
            }
            lookup[num] = j
        }
        return false
    }
}
```