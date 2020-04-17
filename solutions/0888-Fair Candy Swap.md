
### Fair Candy Swap

Alice and Bob have candy bars of different sizes: `A[i]` is the size of the `i`-th bar of candy that Alice has,</br> 
and `B[j]` is the size of the `j`-th bar of candy that Bob has.

Since they are friends, they would like to exchange one candy bar each so that after the exchange,</br>
they both have the same total amount of candy.  
(*The total amount of candy a person has is the sum of the sizes of candy bars they have.*)

Return an integer array `ans` where `ans[0]` is the size of the candy bar that Alice must exchange,</br> 
and `ans[1]` is the size of the candy bar that Bob must exchange.

If there are multiple answers, you may return any one of them.  It is guaranteed an answer exists.

__Example 1:__
```
Input: A = [1,1], B = [2,2]
Output: [1,2]
```
__Example 2:__
```
Input: A = [1,2], B = [2,3]
Output: [1,2]
```
__Example 3:__
```
Input: A = [2], B = [1,3]
Output: [2,3]
```
__Example 4:__
```
Input: A = [1,2,5], B = [2,4]
Output: [5,4]
```

__Note:__
* `1 <= A.length <= 10000`
* `1 <= B.length <= 10000`
* `1 <= A[i] <= 100000`
* `1 <= B[i] <= 100000`
* It is guaranteed that Alice and Bob have different total amounts of candy.
* It is guaranteed there exists an answer.

### Solution
__O(A+B) Time, O(B) Space:__
```Swift
extension Array where Element == Int {
    var sum : Int {
        return reduce(0) { $0 + $1 }
    }
}

class Solution {
    func fairCandySwap(_ A: [Int], _ B: [Int]) -> [Int] {
        let diff = (A.sum-B.sum)/2
        let B = Set(B)
        for num in A {
            let target = num-diff
            if B.contains(target) {
                return [num, target]
            }
        }
        fatalError()
    }
}
```