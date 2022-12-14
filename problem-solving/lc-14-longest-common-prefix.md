<a href="../">Notebook</a> > <a href="./">Problem Solving</a> > LC - 14. Longest Common Prefix

# LC - 14. Longest Common Prefix



## Solutions in C++

### Solution 1

This solution takes the first string in the vector `strs` as a reference, and compares character by character throughout all the strings in `strs`.

**Complexity Analysis:**

*  $T = O(mn)$ where $m$ is the shortest string, $n$ is the number of strings in the given vector `strs` 
*  $S = O(1)$ as no additional memory is needed

**Solution:**

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string ret;

        for (int j = 0; strs[0][j] != '\0'; j++) 
        {
            for (int i = 0; i < strs.size(); i++)
            {
                if (strs[i][j] != strs[0][j])
                    return ret;
            }
            ret += strs[0][j];
        }

        return ret;
    }
};
```

> line 6: Termination condition can also be written as `strs[0].length()`. 

* For the operator `[]`, the position after the last character is valid index. It will return the value that is generated by the default constructor of the character type which is ‘`\0`’.

  However, for `at()`, the position after the last character is NOT valid. It will throw an `out_of_range` exception when it reaches that position.

  ```cpp
  strs[0].at(i);    // throws an exception when i hits the position after the last 
                    // character
  
  strs[0][i];       // returns '\0' when i reaches the position after the last
                    // character
  ```
