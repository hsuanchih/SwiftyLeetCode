
### All Paths From Source to Target

Given a directed acyclic graph (__DAG__) of `n` nodes labeled from `0` to `n - 1`, find all possible paths from node `0` to node `n - 1` and return them in __any order__.

The graph is given as follows: `graph[i]` is a list of all nodes you can visit from node `i` (i.e., there is a directed edge from node i to node `graph[i][j]`).

__Example 1:__

![images/question_797-1.jpg](images/question_797-1.jpg)

```
Input: 
graph = [
    [1,2],
    [3],
    [3],
    []
]

Output: [
    [0,1,3],
    [0,2,3]
]

Explanation: There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.
```

__Example 2:__

![images/question_797-2.jpg](images/question_797-2.jpg)

```
Input: 
graph = [
    [4,3,1],
    [3,2,4],
    [3],
    [4],
    []
]

Output: [
    [0,4],
    [0,3,4],
    [0,1,3,4],
    [0,1,2,3,4],
    [0,1,4]
]
```

__Constraints:__
* `n == graph.length`
* `2 <= n <= 15`
* `0 <= graph[i][j] < n`
* `graph[i][j] != i` (i.e., there will be no self-loops).
* All the elements of `graph[i]` are __unique__.
* The input graph is __guaranteed__ to be a __DAG__.

### Solution
__O(n*n) (V+E), DFS:__
```Swift
class Solution {
    func allPathsSourceTarget(_ graph: [[Int]]) -> [[Int]] {
        guard !graph.isEmpty else { return [] }
        var path: [Int] = [0]
        var visited: Set<Int> = Set(path)
        var result: [[Int]] = []
        walk(node: 0, graph: graph, visited: &visited, path: &path, result: &result)
        return result
    }
    
    // Run DFS on every next nodes that can be reached from node the current node
    func walk(node: Int, graph: [[Int]], visited: inout Set<Int>, path: inout [Int], result: inout [[Int]]) {
        // Node n-1 is reached, add the current path to result
        if node == graph.count-1 {
            result.append(path)
            return
        }
        
        graph[node].forEach {
            if !visited.contains($0) {
                visited.insert($0)
                path.append($0)
                walk(node: $0, graph: graph, visited: &visited, path: &path, result: &result)
                path.removeLast()
                visited.remove($0)
            }
        }
    }
}
```