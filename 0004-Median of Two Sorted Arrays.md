
### Median of Two Sorted Arrays

There are two sorted arrays __nums1__ and __nums2__ of size m and n respectively.</br>
Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).</br>
You may assume __nums1__ and __nums2__ cannot be both empty.

__Example 1:__
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```
__Example 2:__
```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

### Solution
__O((m+n) * log(m+n)) Time, O(m+n) Space:__
```Swift
class Solution {
    func findMedianSortedArrays(_ nums1: [Int], _ nums2: [Int]) -> Double {

        // Combine two arrays & sort
        let sorted = (nums1 + nums2).sorted(), length = sorted.count
        
        // Compute median of the two arrays
        if length % 2 == 1 {
            return Double(sorted[length/2])
        } else {
            return Double(sorted[length/2-1] + sorted[length/2])/2
        }
    }
}
```
__O((m+n)/2) Time, O((m+n)/2) Space:__
```Swift
class Solution {
    func findMedianSortedArrays(_ nums1: [Int], _ nums2: [Int]) -> Double {
        let isEven : Bool = (nums1.count+nums2.count)%2 == 0

        var median = (nums1.count+nums2.count)/2,
        stack : [Int] = [],
        i = 0, 
        j = 0

        // Keep pushing the next smallest value into the stack until median is reached
        while median >= 0 {

            // If both i & j are within array bounds, push the smaller of nums1[i]/nums2[j] into stack
            // If either i or j reaches the end of the array, push the smallest element from the other array into the stack
            switch (i, j) {
                case (0..<nums1.count, 0..<nums2.count):
                if nums1[i] < nums2[j] {
                    stack.append(nums1[i])
                    i+=1
                } else {
                    stack.append(nums2[j])
                    j+=1
                }
                case (nums1.count...Int.max, _):
                stack.append(nums2[j])
                j+=1
                default:
                stack.append(nums1[i])
                i+=1
            }
            median-=1
        }

        // Compute median of the two arrays
        if isEven {
            return Double(stack.removeLast()+stack.removeLast())/2
        } else {
            return Double(stack.removeLast())
        }
    }
}
```