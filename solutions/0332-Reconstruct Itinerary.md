
### Reconstruct Itinerary

Given a list of airline tickets represented by pairs of departure and arrival airports `[from, to]`, reconstruct the itinerary in order. All of the tickets belong to a man who departs from `JFK`. Thus, the itinerary must begin with `JFK`.

__Note:__
1. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].
2. All airports are represented by three capital letters (IATA code).
3. You may assume all tickets form at least one valid itinerary.

__Example 1:__
```
Input: [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
Output: ["JFK", "MUC", "LHR", "SFO", "SJC"]
```
__Example 2:__
```
Input: [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"].
             But it is larger in lexical order.
```

### Solution
```Swift
class Solution {
    func findItinerary(_ tickets: [[String]]) -> [String] {
        var adjList = adjacencyList(tickets), result : [String] = []
        for (key, value) in adjList {
            adjList[key] = value.sorted()
        }
        dfs("JFK", &adjList, &result)
        return result.reversed()
    }
    
    func dfs(_ start: String, _ adjList: inout [String: [String]], _ result: inout [String]) {
        defer { result.append(start) }
        while var nextHops = adjList[start], !nextHops.isEmpty {
            let hop = nextHops.removeFirst()
            adjList[start] = nextHops
            dfs(hop, &adjList, &result)
        }
    }
    
    func adjacencyList(_ tickets: [[String]]) -> [String: [String]] {
        var result : [String: [String]] = [:]
        for ticket in tickets {
            result[ticket[0], default: [String]()].append(ticket[1])
        }
        return result
    }
```