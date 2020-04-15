
### Intersection of Two Arrays II

Given two arrays, write a function to compute their intersection.

__Example 1:__
```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```
__Example 2:__
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```

__Note:__
Each element in the result should appear as many times as it shows in both arrays.
The result can be in any order.

__Follow up:__
* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to nums2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

### Solution
__O(nums1+nums2) Time, O(max(nums1,nums2)) Space:__
```Swift
class Solution {
    func intersect(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var lookup = nums1.reduce(into: [Int: Int]()) { $0[$1, default: 0]+=1 }
        return nums2.reduce(into: [Int]()) {
            if let count = lookup[$1], count > 0 {
                lookup[$1] = count-1
                $0.append($1)
            }
        }
    }
}
```