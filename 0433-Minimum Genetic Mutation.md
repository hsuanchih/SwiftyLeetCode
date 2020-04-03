
### Minimum Genetic Mutation

A gene string can be represented by an 8-character long string, with choices from `"A"`, `"C"`, `"G"`, `"T"`.

Suppose we need to investigate about a mutation (mutation from "start" to "end"), where ONE mutation is defined as ONE single character changed in the gene string.

For example, `"AACCGGTT"` -> `"AACCGGTA"` is 1 mutation.

Also, there is a given gene "bank", which records all the valid gene mutations. A gene must be in the bank to make it a valid gene string.

Now, given 3 things - start, end, bank, your task is to determine what is the minimum number of mutations needed to mutate from "start" to "end". If there is no such a mutation, return -1.

__Note:__
1. Starting point is assumed to be valid, so it might not be included in the bank.
2. If multiple mutations are needed, all mutations during in the sequence must be valid.
3. You may assume start and end string is not the same.
 
__Example 1:__
```
start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

return: 1
```

__Example 2:__
```
start: "AACCGGTT"
end:   "AAACGGTA"
bank: ["AACCGGTA", "AACCGCTA", "AAACGGTA"]

return: 2
```

__Example 3:__
```
start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

return: 3
```

### Solution
__O(bank\*4\*8) Time, O(bank) Space - BFS:__
```Swift
class Solution {
    func minMutation(_ start: String, _ end: String, _ bank: [String]) -> Int {
        let start = Array(start), end = Array(end), mutations : [Character] = ["A", "C", "G", "T"]
        var bank = Set(bank.map(Array.init)), queue : [[Character]] = [start]
        var result = 0
        
        while !queue.isEmpty {
            let length = queue.count
            for _ in 0..<length {
                
                // De-queue each mutation at each level
                var curr = queue.removeFirst()
                
                // If we've reach the ending mutation, solution is found
                if curr == end {
                    return result
                }
                
                // For each mutation at the current level,
                // try the next possible mutations by substituting
                // "A", "C", "G", "T" for each position of the current
                // mutation
                for i in 0..<curr.count {
                    let temp = curr[i]
                    for char in mutations {
                        
                        // If the substition if the same as the current character
                        // we can skip it as it was processed in the previous round
                        if char == temp {
                            continue
                        }
                        
                        // We validate our substitution with the bank, 
                        // and add this new mutation if it is valid
                        curr[i] = char
                        if bank.contains(curr) {
                            queue.append(curr)
                            bank.remove(curr)
                        }
                    }
                    
                    // Restore the current position of the current mutation
                    // as we can only make 1 character change at a time
                    curr[i] = temp
                }
            }
            
            // Increment the number of steps we've taken
            result+=1
        }
        return -1
    }
}
```