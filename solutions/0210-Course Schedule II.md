
### Course Schedule II

There are a total of *n* courses you have to take, labeled from `0` to `n-1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite __pairs__, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

__Example 1:__
```
Input: 2, [[1,0]] 
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished   
             course 0. So the correct course order is [0,1] .
```
__Example 2:__
```
Input: 4, [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3] or [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both     
             courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. 
             So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .
```
__Note:__

1. The input prerequisites is a graph represented by __a list of edges__, not adjacency matrices. Read more about how a graph is represented.
2. You may assume that there are no duplicate edges in the input prerequisites. 

### Solution
__O(V+E):__
```Swift
class Solution {
    func findOrder(_ numCourses: Int, _ prerequisites: [[Int]]) -> [Int] {
        let adjList = adjacencyList(numCourses, prerequisites)
        var visited : Set<Int> = [], visitStack : Set<Int> = [], stack : [Int] = []
        for vertex in 0..<numCourses {
            if !dfs(vertex, adjList, &visited, &visitStack, &stack) {
                return []
            }
        }
        return stack.reversed()
    }
    
    func dfs(_ vertex: Int, _ adjList: [[Int]], _ visited: inout Set<Int>, _ visitStack: inout Set<Int>, _ stack: inout [Int]) -> Bool {
        guard !visitStack.contains(vertex) else { return false }
        visitStack.insert(vertex)
        if !visited.contains(vertex) {
            visited.insert(vertex)
            for next in adjList[vertex] {
                if !dfs(next, adjList, &visited, &visitStack, &stack) {
                    return false
                }
            }
            stack.append(vertex)
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