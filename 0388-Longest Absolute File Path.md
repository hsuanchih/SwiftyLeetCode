
### Longest Absolute File Path

Suppose we abstract our file system by a string in the following manner:

The string `"dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext"` represents:
```
dir
    subdir1
    subdir2
        file.ext
```
The directory `dir` contains an empty sub-directory `subdir1` and a sub-directory `subdir2` containing a file `file.ext`.

The string `"dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"` represents:
```
dir
    subdir1
        file1.ext
        subsubdir1
    subdir2
        subsubdir2
            file2.ext
```

The directory `dir` contains two sub-directories `subdir1` and `subdir2`.</br> 
`subdir1` contains a file `file1.ext` and an empty second-level sub-directory `subsubdir1`.</br> 
`subdir2` contains a second-level sub-directory `subsubdir2` containing a file `file2.ext`.

We are interested in finding the longest (number of characters) absolute path to a file within our file system.</br> 
For example, in the second example above, the longest absolute path is `"dir/subdir2/subsubdir2/file2.ext"`,</br> 
and its length is 32 (not including the double quotes).

Given a string representing the file system in the above format, return the length of the longest absolute path to file in the abstracted file system. If there is no file in the system, return `0`.

__Note:__
* The name of a file contains at least a `.` and an extension.
* The name of a directory or sub-directory will not contain a `.`.

Time complexity required: `O(n)` where `n` is the size of the input string.

Notice that `a/aa/aaa/file1.txt` is not the longest file path, if there is another path `aaaaaaaaaaaaaaaaaaaaa/sth.png`.

### Solution
__O(input):__
```Swift
class Solution {
    func lengthLongestPath(_ input: String) -> Int {
        
        // lvlLength keeps track of the length of each level in the current file path
        // By default we have level -1 with length 0
        var lvlLength : [Int: Int] = [-1: 0], result = 0
        
        // Use "\n" to separate the path components in the input
        // Use "\t" prefix to determine the level of each path component
        // Use "." to distinguish a file 
        for path in input.split(separator: "\n") {
            
            // For each path component we determine the level it is in
            var level = 0, path = Array(path), pathLength = path.count
            while path[level] == "\t" {
                level+=1
            }
            
            // If the component is a file we update result with the longest absolute file path
            // Otherwise we update the cumulative length of the path components up to the current path
            if path.contains(".") {
                result = max(result, pathLength+lvlLength[level-1]!)
            } else {
                lvlLength[level] = lvlLength[level-1]! + pathLength - level
            }
        }
        return result
    }
}
```