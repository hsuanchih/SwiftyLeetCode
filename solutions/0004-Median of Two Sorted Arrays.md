
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
__O((nums1+nums2)*log(nums1+nums2)) Time, O(nums1+nums2) Space - Merge & Sort:__
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
__O((nums1+nums2)+(nums1+nums2)/2) Time, O(nums1+nums2) Space - Modify Input:__
```Swift
class Solution {
    func findMedianSortedArrays(_ nums1: [Int], _ nums2: [Int]) -> Double {
        
        // Reverse nums1 & nums2
        var nums1 = Array(nums1.reversed()), nums2 = Array(nums2.reversed())
        
        // Get the size of the arrays combined
        let size = nums1.count+nums2.count
        
        // Determine how many items we need to pop from nums1 & nums2 combined
        // to get the medium
        var itemsToPop = size/2, curr : Int?
        while itemsToPop > 0 {
            curr = nextSmaller(&nums1, &nums2)
            itemsToPop-=1
        }
        
        // If the number of elements is odd, the median is the middle element
        // Otherwise the median is the average of the middle 2 elements
        if size%2 == 1 {
            return Double(nextSmaller(&nums1, &nums2))
        } else {
            return (Double(curr!)+Double(nextSmaller(&nums1, &nums2)))/2
        }
    }
    
    // Helper method to remove the next smallest element from nums1 & nums2
    func nextSmaller(_ nums1: inout [Int], _ nums2: inout [Int]) -> Int {
        switch (nums1.last, nums2.last) {
            case (.some(_), .none):
            return nums1.removeLast()
            case (.none, .some(_)):
            return nums2.removeLast()
            case let (.some(num1), .some(num2)):
            return num1 < num2 ? nums1.removeLast() : nums2.removeLast()
            default:
            fatalError()
        }
    }
}
```
__O((nums1+nums2)/2) Time, O((nums1+nums2)/2) Space - Preserve Input:__
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
__O(log\[2\](min(nums1,nums2))) Time, O(1) Space - Binary Search:__
```Swift
class Solution {
    func findMedianSortedArrays(_ nums1: [Int], _ nums2: [Int]) -> Double {
        
        // We want to run binary search on the shorter input
        if nums1.count > nums2.count {
            return findMedianSortedArrays(nums2, nums1)
        }

        // Run binary search on nums1
        var start = 0, end = nums1.count
        while start <= end {

            // Identify partition for nums1, then for nums2
            let part1 = start + (end-start)/2, part2 = (nums1.count+nums2.count+1)/2-part1

            // Compute left & min right for both nums1 & nums2
            let left1 = part1 > 0 ? nums1[part1-1] : Int.min, right1 = part1 < nums1.count ? nums1[part1] : Int.max
            let left2 = part2 > 0 ? nums2[part2-1] : Int.min, right2 = part2 < nums2.count ? nums2[part2] : Int.max
            
            // If max left1 <= right2 && left2 <= right1, then we've found our partitions
            if left1 <= right2 && left2 <= right1 {

                // Return median calculation depending on whether the combined array length is even or odd
                if (nums1.count+nums2.count)%2 == 1 {

                    // Median the maximum of the last elements from the left partitions of nums1 & nums2
                    return Double(max(left1, left2))
                } else {

                    // Median is the average of the largest element from the left partitions & the smallest element from the right partitions
                    return Double(max(left1, left2)+min(right1, right2))/2
                }
            }
            
            // If the last element from nums1's left partition is greater than the first element from nums2's right partition,
            // then we've overshot our index selection in nums1, continue searching to the left of nums1
            // Otherwise, search to the right
            if left1 > right2 {
                end = part1-1
            } else {
                start = part1+1
            }
        }
        fatalError()
    }
}
```