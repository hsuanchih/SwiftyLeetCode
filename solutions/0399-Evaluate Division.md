
### Evaluate Division

You are given an array of variable pairs `equations` and an array of real numbers `values`, where `equations[i] = [Ai, Bi]` and `values[i]` represent the equation `Ai / Bi = values[i]`. Each `Ai` or `Bi` is a string that represents a single variable.

You are also given some `queries`, where `queries[j] = [Cj, Dj]` represents the `jth` query where you must find the answer for `Cj / Dj = ?`.

Return the answers to all queries. If a single answer cannot be determined, return `-1.0`.

__Note:__ 
* The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

__Note:__ 
* The variables that do not occur in the list of equations are undefined, so the answer cannot be determined for them.

__Example 1:__
```
Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
Explanation: 
Given: a / b = 2.0, b / c = 3.0
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? 
return: [6.0, 0.5, -1.0, 1.0, -1.0 ]
note: x is undefined => -1.0
```
__Example 2:__
```
Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
Output: [3.75000,0.40000,5.00000,0.20000]
```
__Example 3:__
```
Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
Output: [0.50000,2.00000,-1.00000,-1.00000]
```

__Constraints:__
* `1 <= equations.length <= 20`
* `equations[i].length == 2`
* `1 <= Ai.length, Bi.length <= 5`
* `values.length == equations.length`
* `0.0 < values[i] <= 20.0`
* `1 <= queries.length <= 20`
* `queries[i].length == 2`
* `1 <= Cj.length, Dj.length <= 5`
* `Ai, Bi, Cj, Dj` consist of lower case English letters and digits.

### Solution
__BFS:__
```Swift
class Solution {
    func calcEquation(_ equations: [[String]], _ values: [Double], _ queries: [[String]]) -> [Double] {
        let adjList: [String: [String: Double]] = constructAdjList(equations, values)
        var result: [Double] = []
        for query in queries {
            var visited: Set<String> = [query[0]]
            if let value = solve(query[0], query[1], adjList, &visited) {
                result.append(value)
            } else {
                result.append(-1)
            }
        }
        return result
    }

    func constructAdjList(_ equations: [[String]], _ values: [Double]) -> [String: [String: Double]] {
        var adjList: [String: [String: Double]] = [:]
        for i in 0 ..< equations.count {
            let equation: [String] = equations[i]
            let value: Double = values[i]
            adjList[equation[0], default: [:]][equation[1]] = value
            adjList[equation[1], default: [:]][equation[0]] = 1 / value
        }
        return adjList
    }

    func solve(_ source: String, _ destination: String, _ adjList: [String: [String: Double]], _ visited: inout Set<String>) -> Double? {
        if let keyValues = adjList[source], !keyValues.isEmpty {
            if source == destination {
                return 1
            } else {
                for next in keyValues.keys where !visited.contains(next) {
                    visited.insert(next)
                    if let value = solve(next, destination, adjList, &visited) {
                        return keyValues[next]! * value
                    }
                }
                return nil
            }
        } else {
            return nil
        }
    }
}
```