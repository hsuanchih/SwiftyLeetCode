
### UTF-8 Validation

A character in UTF8 can be from __1 to 4 bytes__ long, subjected to the following rules:

1. For 1-byte character, the first bit is a 0, followed by its unicode code.
2. For n-bytes character, the first n-bits are all one's, the n+1 bit is 0, followed by n-1 bytes with most significant 2 bits being 10.

This is how the UTF-8 encoding would work:
```
   Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```
Given an array of integers representing the data, return whether it is a valid utf-8 encoding.

__Note:__
The input is an array of integers. Only the least significant 8 bits of each integer is used to store the data. This means each integer represents only 1 byte of data.

__Example 1:__
```
data = [197, 130, 1], which represents the octet sequence: 11000101 10000010 00000001.

Return true.
It is a valid utf-8 encoding for a 2-bytes character followed by a 1-byte character.
```
__Example 2:__
```
data = [235, 140, 4], which represented the octet sequence: 11101011 10001100 00000100.

Return false.
The first 3 bits are all one's and the 4th bit is 0 means it is a 3-bytes character.
The next byte is a continuation byte which starts with 10 and that's correct.
But the second continuation byte does not start with 10, so it is invalid.
```

### Solution
__O(data) Time, O(1) Space:__
```Swift
class Solution {
    func validUtf8(_ data: [Int]) -> Bool {
        var i = 0
        while i < data.count {
            switch numBytes(data[i]) {
                case 1:
                i+=1
                case Int.max:
                return false
                case let bytes:
                let nextStart = i+bytes
                if nextStart > data.count {
                    return false
                }
                for j in i+1...nextStart-1 {
                    if !validate(data[j]) {
                        return false
                    }
                }
                i = nextStart
            }
        }
        return true
    }
    
    // Helper method to validate whether a 8-bit number has MSBs "10"
    func validate(_ num: Int) -> Bool {
        let num = UInt8(num)
        return num & 0b10000000 == 0b10000000
    }
    
    // Helper method to compute the length of the next UTF character
    func numBytes(_ num: Int) -> Int {
        let num = UInt8(num)

        // If MSB is 0, 1 byte character
        if num & 0b10000000 == 0 {
            return 1
        }

        // Check for 2/3/4 byte characters
        var mask : UInt8 = 0b11111000
        for shift in 0...2 {
            if num & mask == mask << 1 {
                return 4-shift
            }
            mask <<= 1
        }

        // num has invalid prefix
        return Int.max
    }
}
```