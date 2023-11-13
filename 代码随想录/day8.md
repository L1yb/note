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



## [151. 反转字符串中的单词 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-words-in-a-string/)

**利用额外空间：**

- 除去开头空格（迭代器+erase）

- 除去结尾空格（迭代器+erase）
- 除去中间连续空格（遍历）

反转的逻辑：

将处理过后的字符串从后往前遍历，每遇到一个空格代表获取读取一个完整的单词，将单词写入新数组，就实现了反转。

具体来说：append（string），接受string参数，而substr（pos，len）返回值是string，中间遇到‘ ’空格就截取，然后补充一个空格，循环控制i >= 1

为什么不取到0呢？

1. 因为已经去除开头空格了，0索引必然是字母，所以不用判断是否为空格。

2. 当索引到0时，后面不再需要补充空格了，开头元素被放到了result的结尾，程序可以结束了



```cpp
class Solution {
public:
    string reverseWords(string s) {
        string result ;
        //除去开头空格
        while ( *s.begin() == ' ') {
            s.erase(s.begin());
        }
        //除去结尾空格
        while( *(s.begin() + s.size() - 1) == ' ') {
            s.erase(s.begin() + s.size() - 1);            
        }
        for (int i = 0; i < s.size() - 1; i++) {
            //去除中间连续空格
            if (s[i] == ' ' && s[i + 1] == ' ') {
                s.erase(s.begin() + i); 
                i--;
            }
        }
        int flag = s.size();
        for (int i = s.size() - 1; i > 0; i--) {
            if (s[i - 1] == ' ') {
                result.append(s.substr(i,flag - i));
                result.append(" ");
                flag = i - 1;
            }
        }
        result.append(s.substr(0, flag));
        return result;
    }
};
```

**不使用赋值空间，在原字符串上修改**

思路：

1. 去除多余空格
2. 反转整个字符串，使用reverse（），语序和单词都反过来了。
3. 正向遍历，每次遇到空格，或者遍历到结尾，将字串再次反转，单词就正过来了



















