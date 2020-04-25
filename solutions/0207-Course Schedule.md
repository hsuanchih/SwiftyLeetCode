
### Course Schedule

There are a total of *n* courses you have to take, labeled from `0` to `n-1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite __pairs__, is it possible for you to finish all courses?

__Example 1:__
```
Input: 2, [[1,0]] 
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
```
__Example 2:__
```
Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```
__Note:__

1. The input prerequisites is a graph represented by a __list of edges__, not adjacency matrices. Read more about how a graph is represented.
2. You may assume that there are no duplicate edges in the input prerequisites.

### Solution
__O(V+E):__
```Swift
class Solution {
    func canFinish(_ numCourses: Int, _ prerequisites: [[Int]]) -> Bool {
        let adjList = adjacencyList(numCourses, prerequisites)
        var visited : Set<Int> = [], visitStack : Set<Int> = []
        for vertex in 0..<numCourses-1 {
            if !dfs(vertex, adjList, &visited, &visitStack) {
                return false
            }
        }
        return true
    }
    
    func dfs(_ vertex: Int, _ adjList: [[Int]], _ visited: inout Set<Int>, _ visitStack: inout Set<Int>) -> Bool {
        guard !visitStack.contains(vertex) else { return false }
        visitStack.insert(vertex)
        if !visited.contains(vertex) {
            visited.insert(vertex)
            for next in adjList[vertex] {
                if !dfs(next, adjList, &visited, &visitStack) {
                    return false
                }
            }
        }
        visitStack.remove(vertex)
        return true
    }
    
    func adjacencyList(_ numCourses: Int, _ prerequisites: [[Int]]) -> [[Int]] {
        var result : [[Int]] = Array(repeating: [Int](), count: numCourses)
        prerequisites.forEach {
            result[$0[1]].append($0[0])
        }
        return result
    }
}
```