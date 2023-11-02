## [541. 反转字符串 II - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string-ii/)

双指针，分别指向交换双方，然后调用swap函数，没有使用**库函数**的意识，

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int i = 0,j = s.size() - 1; i < j; i++,j--)  { 
            swap(s[i], s[j]);
        }
        
    }
};
```



## [541. 反转字符串 II - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string-ii/)

在上一题基础上，不同之处是，上一题是完整的一个字符串，现在是串中的一部分，之前错误的写法是使用substr库函数去截取，但是题目要求的是**在原字符串修改**，截取出字串没有意义。

使用自定义的reverse函数：

**修改reverse函数**，指定长度，如果使用闭区间，需要弄清起点和终点，步长是k，那么就是 [i，i + k - 1]


```cpp
class Solution {
public:
    void reverse(string &s, int begin, int end) {
        for (int i = begin, j = end; i < j; i++,j--) {
            swap(s[i], s[j]);
        }
    }
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i = i + 2 * k) {
            if (i + k <= s.size()) {
                reverse(s, i, i + k - 1);
            }
            else {
                reverse(s, i, s.size() - 1);
            }

        }
        return s;

    }
};
```

使用标准库reverse()函数：

- 参数是迭代器

```cpp
class Solution {
public:

    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i = i + 2 * k) {
            if (i + k <= s.size()) {
                // reverse(s, i, i + k - 1);
                reverse(s.begin() + i, s.begin() + i + k);
            }
            else {
                // reverse(s, i, s.size() - 1);
                reverse(s.begin() + i, s.end());

            }

        }
        return s;

    }
};
```

































