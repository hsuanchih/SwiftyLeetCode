
### Merge Sorted Array

Given two sorted integer arrays *nums1* and *nums2*, merge *nums2* into *nums1* as one sorted array.

__Note:__

The number of elements initialized in *nums1* and *nums2* are *m* and *n* respectively.
You may assume that *nums1* has enough space (size that is greater or equal to *m* + *n*) to hold additional elements from *nums2*.

__Example:__
```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```

### Solution
__O(m+n):__
```Swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        var i = m-1, j = n-1, end = nums1.count-1
        while i >= 0 && j >= 0 {
            if nums1[i] > nums2[j] {
                nums1[end] = nums1[i]
                i-=1
            } else {
                nums1[end] = nums2[j]
                j-=1
            }
            end-=1
        }
        while i >= 0 {
            nums1[end] = nums1[i]
            i-=1
            end-=1
        }
        while j >= 0 {
            nums1[end] = nums2[j]
            j-=1
            end-=1
        }
    }
}
```