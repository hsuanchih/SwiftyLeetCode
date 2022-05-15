
### Maximum Product Subarray

Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.

__Example 1:__
```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```
__Example 2:__
```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

__Constraints:__
* `1 <= nums.length <= 2 * 10^4`
* `-10 <= nums[i] <= 10`
* The product of any prefix or suffix of `nums` is __guaranteed__ to fit in a __32-bit__ integer.

### Solution
__O(nums^2) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func maxProduct(_ nums: [Int]) -> Int {
        var result = Int.min
        for start in 0..<nums.count {
            var product = 1
            for end in start..<nums.count {
                product*=nums[end]
                result = max(product, result)
            }
        }
        return result
    }
}
```
__O(2*nums) Time - O(1) Space:__
```Swift
class Solution {
    func maxProduct(_ nums: [Int]) -> Int {
        var maxConsecutiveNonNegativeProduct: Int = 1
        var product: Int = 1
        var result: Int = Int.min
        for num in nums {
            evaluate(num, &maxConsecutiveNonNegativeProduct, &product, &result)
        }
        maxConsecutiveNonNegativeProduct = 1
        product = 1
        for i in stride(from: nums.count-1, through: 0, by: -1) {
            evaluate(nums[i], &maxConsecutiveNonNegativeProduct, &product, &result)
        }
        return result
    }
    
    func evaluate(_ num: Int, _ maxConsecutiveNonNegativeProduct: inout Int, _ product: inout Int, _ result: inout Int) {
        switch num {
        case 0:
            maxConsecutiveNonNegativeProduct = 1
            product = 1
            result = max(num, result)
        case Int.min ..< 0:
            maxConsecutiveNonNegativeProduct = 1
            product *= num
            result = max(product, result)
        case 1...:
            maxConsecutiveNonNegativeProduct *= num
            product *= num
            result = max(result, max(maxConsecutiveNonNegativeProduct, product))
        default:
            break
        }
    }
}
```
__O(nums) Time - O(1) Space - Prefix-Product/Memoization:__
```Swift
class Solution {
    func maxProduct(_ nums: [Int]) -> Int {
        var maxProduct = 1, minProduct = 1, result = Int.min
        for num in nums {
            let prefixMax = max(maxProduct*num, minProduct*num),
            prefixMin = min(maxProduct*num, minProduct*num)
            maxProduct = max(prefixMax, num)
            minProduct = min(prefixMin, num)
            result = max(result, maxProduct)
        }
        return result
    }
}
```