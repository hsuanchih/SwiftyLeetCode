
### Min Cost Climbing Stairs

On a staircase, the `i`-th step has some non-negative cost `cost[i]` assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps.</br> 
You need to find minimum cost to reach the top of the floor,</br> 
and you can either start from the step with index 0, or the step with index 1.

__Example 1:__
```
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
```
__Example 2:__
```
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
```

__Note:__
1. `cost` will have a length in the range `[2, 1000]`.
2. Every `cost[i]` will be an integer in the range `[0, 999]`.

### Solution
__O(2^cost) Time, O(1) space, TLE:__
```swift
class Solution {
    func minCostClimbingStairs(_ cost: [Int]) -> Int {
        return minCost(cost.count, cost: cost)
    }
    
    func minCost(_ n: Int, cost: [Int]) -> Int {
        // Costs nothing to reach steps 0 or 1
        guard n > 1 else { return 0 }

        // Otherwise the cost to reach step n is the minimum of:
        // * The min cost it took to reach step n-1 + the cost to go from step n-1 to step n
        // * The min cost it took to reach step n-2 + the cost to go from step n-2 to step n
        return min(
            cost[n-1] + minCost(n-1, cost: cost), 
            cost[n-2] + minCost(n-2, cost: cost)
        )
    }
}
```

__O(cost) Time, O(cost) Space (in-place modify):__
```swift
class Solution {
    func minCostClimbingStairs(_ cost: [Int]) -> Int {
        switch cost.count {
        case 0: return 0
        case 1: return cost.first!
        default: break
        }
        var cost = cost
        for (index, _) in cost.enumerated() {
            if index < 2 {
                continue
            }
            cost[index] += min(cost[index-1], cost[index-2])
        }
        return min(cost.last!, cost[cost.count-2])
    }
}
```

__O(cost) Time, O(1) Space:__
```swift
class Solution {
    func minCostClimbingStairs(_ cost: [Int]) -> Int {
        guard cost.count >= 2 else { return 0 }
        var minus1: Int = 0, minus2: Int = 0
        (2 ... cost.count).forEach {
            let current = min(cost[$0-1] + minus1, cost[$0-2] + minus2)
            minus2 = minus1
            minus1 = current
        }
        return minus1
    }
}
```