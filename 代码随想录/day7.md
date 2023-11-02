## [454. 四数相加 II - 力扣（LeetCode）](https://leetcode.cn/problems/4sum-ii/)

我想的是把数组1、2之和放入map1中，然后把3、4之和放入map2中，再遍历map1，在map2中find map1元素的负数

正确结果和这个想法有相似的地方，但是更为简洁，规范

- 将数组1、2之和放入map，value的值存放出现的次数，也是组合的方法数

- 数组3、4之和的负数，也就**是map的key值**，使用这个来索引，如果找得到，对应value的值就是组合数
- 定义count来计数



```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> sumMap;//nums1,2
        for (int a : nums1) {
            for (int b : nums2) {
                sumMap[a + b]++; 
            }
        }
        int count = 0;
        for (int c : nums3) {
            for (int d : nums4) {
                count += sumMap[0 - (c + d)];
            }
        }
        return count;
        
    }
};
```

## [383. 赎金信 - 力扣（LeetCode）](https://leetcode.cn/problems/ransom-note/)

很简单，记录已有元素以及数目，合适的数据类型为unordered_map

key为字母，value为数目

这样就建立个一个字典库

看能不能组成目标字符串只需要看两个条件：

1. 字典是否有这个字母
2. 需要字母的数目是否大于拥有的数目

此外，随想录给出的答案中，将字母转化为数字，**使用数组**vector<int> vec(26,0),数据结构更为简单，以后关于字母可以使用数组解决

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int> dirMap;
        for (char c : magazine) {
            dirMap[c]++;
        }
        for (char r : ransomNote) {
            if (dirMap.find(r) == dirMap.end()) {
                return false;
            }
            
            if (--dirMap[r] < 0) {
                return false;
            } 
        }
        return true;
    }
};
```

## [15. 三数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/3sum/description/)