
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
__Example 3:__
```
Input: s = "A", numRows = 1
Output: "A"
```

__Constraints:__
* `1 <= s.length <= 1000`
* `s` consists of English letters (lower-case and upper-case), `','` and `'.'`.
* `1 <= numRows <= 1000`

### Solution
__O(s) Time, O(1) Space:__
```Swift
class Solution {
    func convert(_ s: String, _ numRows: Int) -> String {
        if numRows == 1 || s.count == numRows {
            return s
        } else {
            let s: [Character] = Array(s) 
            var result: String = ""

            // Iterate through rows 0 ..< numRows and map the characters in s to each row
            for row in 0 ..< numRows {
                var i: Int = row
                var isDownUp: Bool = true
                while i < s.count {
                    result.append(s[i])
                    if row == 0 {
                        // For row 0 we're always going from top to bottom
                        i += 2 * ((numRows - 1) - row)
                    } else if row == numRows - 1 {
                        // For row numRows - 1 we're always going from bottom to top
                        i += 2 * row
                    } else {
                        if isDownUp {
                            i += 2 * ((numRows - 1) - row)
                        } else {
                            i += 2 * row
                        }
                        isDownUp.toggle()
                    }
                }
            }
            return result
        }
    }
}
```