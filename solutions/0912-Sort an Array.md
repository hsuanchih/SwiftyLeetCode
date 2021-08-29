
### Sort an Array

Given an array of integers `nums`, sort the array in ascending order.

__Example 1:__
```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]
```
__Example 2:__
```
Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]
```

__Constraints:__
* `1 <= nums.length <= 50000`
* `-50000 <= nums[i] <= 50000`

### Solution
__Counting Sort | O(n+Range(n)) Time, O(Range(n)) Space:__
```Swift
class Solution {
    func sortArray(_ nums: [Int]) -> [Int] {
        var nums = nums, tally : [Int] = Array(repeating: 0, count: 50000*2+1)
        for num in nums {
            tally[num+50000]+=1
        }
        var j = 0
        for i in 0..<tally.count {
            let count = tally[i]
            for _ in 0..<count {
                nums[j] = i-50000
                j+=1
            }
        }
        return nums
    }
}
```
__Merge Sort | O(n*log(n)) Time, O(n) Space:__
```Swift
class Solution {
    func sortArray(_ nums: [Int]) -> [Int] {
        var nums = nums
        split(&nums, 0, nums.count-1)
        return nums
    }
    
    // Continue splitting nums down the middle until each sub-array is of size 1
    // Sort & merge the sub-arrays from start to end
    func split(_ nums: inout [Int], _ start: Int, _ end: Int) {
        if start >= end {
            return
        }
        let mid = start + (end-start)/2
        split(&nums, start, mid)
        split(&nums, mid+1, end)
        mergeSort(&nums, start, mid, end)
    }
    
    // The first sub-array starts at index start & ends at index mid
    // The second sub-array starts at index mid+1 & ends at index end
    // Sort & merge the sub-arrays
    func mergeSort(_ nums: inout [Int], _ start: Int, _ mid: Int, _ end: Int) {
        var left = start, right = mid+1, i = 0
        var result = Array(repeating: 0, count: end-start+1)
        while left <= mid && right <= end {
            if nums[left] < nums[right] {
                result[i] = nums[left]
                left+=1
            } else {
                result[i] = nums[right]
                right+=1
            }
            i+=1
        }
        while left <= mid {
            result[i] = nums[left]
            left+=1
            i+=1
        }
        while right <= end {
            result[i] = nums[right]
            right+=1
            i+=1
        }
        i = start
        for num in result {
            nums[i] = num
            i+=1
        }
    }
}
```
__Quick Sort | O(n*log(n)~O(n^2)) Time, O(1) Space:__
```Swift
class Solution {
    func sortArray(_ nums: [Int]) -> [Int] {
        var nums = nums
        quickSort(&nums, 0, nums.count - 1)
        return nums
    }

    // Continue splitting nums using the pivot until each sub-array is of size 1
    func quickSort(_ nums: inout [Int], _ start: Int, _ end: Int) {
        if start >= end {
            return
        }

        // 1. Randomly choose an element as the pivot in sub-array nums[start ... end]
        // 2. Move all elements with value less than the pivot to its left 
        //   (all elements with value greater will naturally end up on its right)
        // 3. Identify the correct position of the pivot in sub-array nums[start ... end]
        let pivot = partition(&nums, start, end)
        
        // Run quick sort on the (unsorted) sub-arrays to the left & right of the pivot.
        quickSort(&nums, start, pivot - 1)
        quickSort(&nums, pivot + 1, end)
    }
    
    // Pick a random element (nums[end]) as the pivot, and return the index the pivot in the sub-array
    func partition(_ nums: inout [Int], _ start: Int, _ end: Int) -> Int {
        
        // Elements with value less than pivot are placed to the left of i (the divider)
        var i = start

        // Randomly pick a pivot in sub-array nums[start ... end], here we pick nums[end].
        // Iterate through the array using j (the iterator), if element is less than pivot
        // swap elements at i & j, then advance i
        for j in start ... end {
            if nums[j] <= nums[end] {
                if i != j {
                    nums.swapAt(i, j)
                }
                i+=1
            }
        }

        // Return the index of the pivot
        return i-1
    }
}
```
