
### String Compression

Given an array of characters, compress it in-place.</br>
The length after compression must always be smaller than or equal to the original array.</br>
Every element of the array should be a __character__ (not int) of length 1.</br>
After you are done __modifying the input array in-place__, return the new length of the array.

__Follow up:__
Could you solve it using only O(1) extra space?

__Example 1:__
```
Input:
["a","a","b","b","c","c","c"]

Output:
Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]

Explanation:
"aa" is replaced by "a2". "bb" is replaced by "b2". "ccc" is replaced by "c3".
```
__Example 2:__
```
Input:
["a"]

Output:
Return 1, and the first 1 characters of the input array should be: ["a"]

Explanation:
Nothing is replaced.
```
__Example 3:__
```
Input:
["a","b","b","b","b","b","b","b","b","b","b","b","b"]

Output:
Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].

Explanation:
Since the character "a" does not repeat, it is not compressed. "bbbbbbbbbbbb" is replaced by "b12".
Notice each digit has it's own entry in the array.
```

### Solution
__O(chars) Time, O(1) Space - Read & Write Heads:__
```Swift
extension Int {
    var characters : [Character] {
        return Array(String(self))
    }
}

class Solution {
    func compress(_ chars: inout [Character]) -> Int {
        var write = 0, curr = 0
        for read in 0...chars.count {
            if read != chars.count && chars[read] == chars[curr] {
                continue
            }
            chars[write] = chars[curr]
            write+=1
            switch (read-curr).characters {
                case ["1"]:
                break
                case let num:
                for char in num {
                    chars[write] = char
                    write+=1
                }
            }
            curr = read
        }
        return write
    }
}
```