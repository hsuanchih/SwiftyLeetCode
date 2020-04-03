
### Binary Watch

A binary watch has 4 LEDs on the top which represent the __hours (0-11)__, and the 6 LEDs on the bottom represent the __minutes (0-59)__.

Each LED represents a zero or one, with the least significant bit on the right.

![Example](images/question_401.jpg)

For example, the above binary watch reads "3:25".

Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.

__Example:__
```
Input: n = 1
Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
```

__Note:__
* The order of output does not matter.
* The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".
* The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".

### Solution
__O(12\*60\*(2\*6)) Time, O(1) Space:__
```Swift
class Solution {
    func readBinaryWatch(_ num: Int) -> [String] {
        var result : [String] = []
        for hour in 0...11 {
            for minute in 0...59 {
                if bits(hour)+bits(minute) == num {
                    result.append(String(format:"%d:%02d", hour, minute))
                }
            }
        }
        return result
    }
    
    func bits(_ num: Int) -> Int {
        var count = 0
        for i in 0..<6 {
            count += (num>>i & 1)
        }
        return count
    }
}
```