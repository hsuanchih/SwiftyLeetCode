
### Count and Say

The count-and-say sequence is the sequence of integers with the first five terms as following:
```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```
`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2, then one 1"` or `1211`.

Given an integer *n* where 1 ≤ *n* ≤ 30, generate the nth term of the count-and-say sequence.</br> 
You can do so recursively, in other words from the previous member read off the digits, counting the number of digits in groups of the same digit.

Note: Each term of the sequence of integers will be represented as a string.

__Example 1:__
```
Input: 1
Output: "1"
Explanation: This is the base case.
```
__Example 2:__
```
Input: 4
Output: "1211"
Explanation: For n = 3 the term was "21" in which we have two groups "2" and "1", "2" can be read as "12" which means frequency = 1 and value = 2, the same way "1" is read as "11", so the answer is the concatenation of "12" and "11" which is "1211".
```

### Solution
__Recursive:__
```Swift
class Solution {
    func countAndSay(_ n: Int) -> String {
        if n == 1 {
            return "\(n)"
        }
        let prev = countAndSay(n-1)
        var count : Int = 0, curr : Character = Character("X"), result : String = ""
        for char in prev {
            if char == curr  {
                count += 1
            } else {
                if curr != "X" {
                    result.append("\(count)\(curr)")
                }
                count = 1
                curr = char
            }
        }
        result.append("\(count)\(curr)")
        return result
    }
}
```
__Iterative:__
```Swift
class Solution {
    func countAndSay(_ n: Int) -> String {
        var count: Int = 1, result : String = "1"
        for _ in stride(from: 1, to: n, by: 1) {
            var count : Int = 0, curr : Character = Character("X"), temp : String = ""
            for char in result {
                if char == curr {
                    count+=1
                } else {
                    if curr != "X" {
                        temp.append("\(count)\(curr)")
                    }
                    curr = char
                    count = 1
                }
            }
            temp.append("\(count)\(curr)")
            result = temp
        }
        return result
    }
}
```