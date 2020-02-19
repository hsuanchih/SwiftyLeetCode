
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
    typealias AdjacencyList = [Int: Set<Int>]
    
    func canFinish(_ numCourses: Int, _ prerequisites: [[Int]]) -> Bool {
        let adjacencyList : AdjacencyList = constructAdjacencyList(prerequisites)
        var stack : [Int] = [], visited : Set<Int> = []
        for course in adjacencyList.keys {
            if !topologicalSort(course, adjacencyList, &stack, &visited) {
                return false
            }
        }
        return true
    }
    
    func topologicalSort(_ course : Int, _ adjacencyList: AdjacencyList, _ stack: inout [Int], _ visited: inout Set<Int>) -> Bool {
        if stack.contains(course) {
            return false
        }
        stack.append(course)
        if !visited.contains(course) {
            visited.insert(course)
            for prereq in adjacencyList[course] ?? [] {
                if !topologicalSort(prereq, adjacencyList, &stack, &visited) {
                    return false
                }
            }
        }
        stack.removeLast()
        return true
    }
    
    func constructAdjacencyList(_ prereq: [[Int]]) -> AdjacencyList {
        return prereq.reduce(into: AdjacencyList()) {
            $0[$1.first!, default: Set<Int>()].insert($1.last!)
        }
    }
}
```