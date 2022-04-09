
### Shortest Path with Alternating Colors

You are given an integer `n`, the number of nodes in a directed graph where the nodes are labeled from `0` to `n - 1`. Each edge is red or blue in this graph, and there could be self-edges and parallel edges.

You are given two arrays `redEdges` and `blueEdges` where:

* `redEdges[i] = [ai, bi]` indicates that there is a directed red edge from node `ai` to node `bi` in the graph, and
* `blueEdges[j]` = `[uj, vj]` indicates that there is a directed blue edge from node `uj` to node `vj` in the graph.

Return an array `answer` of length `n`, where each `answer[x]` is the length of the shortest path from node `0` to node `x` such that the edge colors alternate along the path, or `-1` if such a path does not exist.

__Example 1:__
```
Input: 
n = 3, 
redEdges = [[0,1],[1,2]], 
blueEdges = []

Output: [0,1,-1]
```

__Example 2:__
```
Input: 
n = 3, 
redEdges = [[0,1]], 
blueEdges = [[2,1]]

Output: [0,1,-1]
```

__Constraints:__
* `1 <= n <= 100`
* `0 <= redEdges.length, blueEdges.length <= 400`
* `redEdges[i].length == blueEdges[j].length == 2`
* `0 <= ai, bi, uj, vj < n`

### Solution
__O(2*(redEdges+blueEdges)), BFS:__
```swift
class Solution {
    func shortestAlternatingPaths(_ n: Int, _ redEdges: [[Int]], _ blueEdges: [[Int]]) -> [Int] {
        var result: [Int] = Array(repeating: Int.max, count: n)
        let redPath = processEdges(n: n, edges: redEdges)
        let bluePath = processEdges(n: n, edges: blueEdges)
        [true, false].forEach {
            walk(startWithRed: $0, redPath: redPath, bluePath: bluePath, result: &result)
        }
        return result.map { $0 == Int.max ? -1 : $0 }
    }
    
    func walk(startWithRed: Bool, redPath: [Set<Int>], bluePath: [Set<Int>], result: inout [Int]) {
        var isRed: Bool = startWithRed
        var queue: [Int] = [0]
        var visitedRed: Set<Int> = [], visitedBlue: Set<Int> = []
        if isRed {
            visitedRed.insert(0)
        } else {
            visitedBlue.insert(0)
        }
        var length: Int = 0
        while !queue.isEmpty {
            let count = queue.count
            for _ in 0 ..< count {
                let node = queue.removeFirst()
                result[node] = min(length, result[node])
                if isRed {
                    bluePath[node].forEach { next in
                        if !visitedBlue.contains(next) {
                            visitedBlue.insert(next)
                            queue.append(next)
                        }
                    }
                } else {
                    redPath[node].forEach { next in
                        if !visitedRed.contains(next) {
                            visitedRed.insert(next)
                            queue.append(next)
                        }
                    }
                }
            }
            length += 1
            isRed.toggle()
        }
    }
    
    func processEdges(n: Int, edges: [[Int]]) -> [Set<Int>] {
        var result: [Set<Int>] = Array(repeating: [], count: n)
        edges.forEach { edge in
            result[edge.first!].insert(edge.last!)
        }
        return result
    }
}
```