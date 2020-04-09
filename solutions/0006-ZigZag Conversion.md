
### ZigZag Conversion

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this:</br> 
(you may want to display this pattern in a fixed font for better legibility)
```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:
```
string convert(string s, int numRows);
```
__Example 1:__
```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```
__Example 2:__
```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

### Solution
__O(s) Time, O(1) Space:__
```Swift
class Solution {
    func convert(_ s: String, _ numRows: Int) -> String {

        // If numRows == 1, return s
        guard numRows > 1 else { return s } 
        var result = ""
        let s = Array(s)

        // Iterate through rows 0..<numRows and map the characters in s to each row
        for i in 0..<numRows {
            var j = i, toggle = false
            while j < s.count {
                result.append(s[j])
                switch i {

                    // If we're in row 0 or the last row, the next character is going to be 2*(numRows-1) indices from the currrent
                    case 0:
                    j += 2*(numRows-1)
                    case numRows-1:
                    j += i*2

                    // All the next indices for the inner rows are going to alternate between offsets 2*(numRows-1-i) & 2*i
                    // Use a toggle to alternate between the 2 offsets
                    default:
                    j += (toggle ? 2*i : 2*(numRows-1-i))
                    toggle = !toggle
                }
            }
        }
        return result
    }
}
```