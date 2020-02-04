
### Letter Combinations of a Phone Number

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.</br>
A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

__Example:__
```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

### Solution
__O(3^m*4^n):__
```Swift
class Solution {
    
    // Phone key to corresponding letters lookup
    let lettersForKey : [Character : String] = [
        "2": "abc",
        "3": "def",
        "4": "ghi",
        "5": "jkl",
        "6": "mno",
        "7": "pqrs",
        "8": "tuv",
        "9": "wxyz",
    ]
    
    func letterCombinations(_ digits: String) -> [String] {
        var temp : String = "", result : [String] = []
        generate(Array(digits), 0, &temp, &result)
        return result
    }
    
    func generate(_ digits: [Character], _ index: Int, _ temp: inout String, _ result: inout [String]) {

        // If index reaches digit count, we know we've reached a possible solution,
        // add the solution to result
        if index == digits.count {

            // if input is empty String, result is an empty set
            if !digits.isEmpty {
                result.append(temp)
            }
            return
        }

        // Lookup letters corresponding to a phone key
        let letters = Array(lettersForKey[digits[index]]!)

        // Exhaust all combinations of letters for a particular key
        for i in stride(from: 0, to: letters.count, by: 1) {
            temp.append(letters[i])
            generate(digits, index+1, &temp, &result)
            temp.removeLast()
        }
    }
}
```
