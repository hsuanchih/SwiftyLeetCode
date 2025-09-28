
### Gas Station

There are `n` gas stations along a circular route, where the amount of gas at the ith station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `ith` station to its next `(i + 1)th` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return the starting gas station's `index` if you can travel around the circuit once in the clockwise direction, otherwise return `-1`. If there exists a solution, it is guaranteed to be unique

__Example 1:__
```
Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```
__Example 2:__
```
Input: gas = [2,3,4], cost = [3,4,3]
Output: -1
Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

__Constraints:__
* `n == gas.length == cost.length`
* `1 <= n <= pow(10, 5)`
* `0 <= gas[i], cost[i] <= pow(10, 4)`

### Solution
__O(pow(gas, 2)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func canCompleteCircuit(_ gas: [Int], _ cost: [Int]) -> Int {
        for start in 0 ..< gas.count {
            var tank: Int = 0
            for i in 0 ..< gas.count {
                let stop: Int = (start + i) % gas.count
                tank += gas[stop] - cost[stop]
                if tank < 0 {
                    break
                } else if i == gas.count - 1 {
                    return start
                }
            }
        }
        return -1
    }
}
```
__O(gas) Time, O(1) Space - Greedy:__
```Swift
class Solution {
    func canCompleteCircuit(_ gas: [Int], _ cost: [Int]) -> Int {
        var tank: Int = 0
        var distance: Int = 0

        // Check if we can go gas.count without running out of gas from every starting point 
        // (from 0 to gas.count - 1)
        for index in 0 ..< (gas.count * 2) {
            let station = index % gas.count
            // Fill the gas & subtract the cost at every station
            tank = tank + gas[station] - cost[station]
            if tank < 0 {
                // If tank goes below 0, we'll not be able travel full circle
                // Use the next station as a new starting point
                distance = 0
                tank = 0
            } else {
                // Otherwise keep track of the distance we've travelled
                distance += 1
            }

            // If we've travelled gas.count stations, then we've travelled full circle.
            // Return the station from which we started
            if distance == gas.count {
                return index - distance + 1
            }
        }
        // We cannot travel full circle, return -1
        return -1
    }
}
```
__O(gas) Time, O(1) Space - Greedy:__
```Swift
class Solution {
    func canCompleteCircuit(_ gas: [Int], _ cost: [Int]) -> Int {
        var start: Int = 0
        while start < gas.count {
            var tank: Int = 0
            for i in 0 ..< gas.count {
                let stop: Int = (start + i) % gas.count
                tank += gas[stop] - cost[stop]
                if tank < 0 {
                    start = start + i + 1
                    break
                } else if i == gas.count - 1 {
                    return start
                }
            }
        }
        return -1
    }
}
```
__O(gas) Time, O(1) Space - Greedy:__
```Swift
class Solution {
    func canCompleteCircuit(_ gas: [Int], _ cost: [Int]) -> Int {
        var tank: Int = 0
        var station: Int = 0
        for i in 0 ..< gas.count * 2 {
            let index = i % gas.count
            tank += gas[index] - cost[index]
            if tank < 0 {
                station = i + 1
                tank = 0
            }
            if station >= gas.count {
                return -1
            }
        }
        return station
    }
}
```