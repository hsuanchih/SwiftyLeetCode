
### Intersection of Two Arrays

Given two arrays, write a function to compute their intersection.

__Example 1:__
```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```
__Example 2:__
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

__Note:__
* Each element in the result must be unique.
* The result can be in any order.

### Solution
__O(nums1+nums2):__
```Swift
class Solution {
    func intersection(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        let lookup = nums1.reduce(into: Set<Int>()) { $0.insert($1) }
        return Array(
            nums2.reduce(into: Set<Int>()) { 
                if lookup.contains($1) {
                    $0.insert($1)
                }
            }
        )
    }
}
```