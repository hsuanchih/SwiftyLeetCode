
### Next Permutation

Implement __next permutation__, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

### Solution
__O(n):__
```Swift
extension Array where Element : Comparable {
    var pivot : Int {
        guard !isEmpty else { return -1 }
        var i = count-2
        while i >= 0 && self[i] >= self[i+1] {
            i-=1
        }
        return i
    }
    
    func indexOfFirstElementGreaterThan(_ element: Element) -> Int {
        var i = count-1
        while i >= 0 && self[i] <= element {
            i-=1
        }
        return i
    }
    
    mutating func reverse(_ start: Int, _ end: Int) {
        var left = start, right = end
        while left < right {
            swapAt(left, right)
            left+=1
            right-=1
        }
    }
}

class Solution {
    func nextPermutation(_ nums: inout [Int]) {
        let pivot = nums.pivot
        if pivot >= 0 {
            let nextGreater = nums.indexOfFirstElementGreaterThan(nums[pivot])
            nums.swapAt(pivot, nextGreater)
        }
        nums.reverse(pivot+1, nums.count-1)
    }
}
```