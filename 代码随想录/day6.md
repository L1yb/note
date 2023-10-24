# 哈希

## 哈希表Hash table

> 哈希表是根据关键码的值直接进行访问的数据结构
>
> 数组就是一张哈希表

**哈希表用来快速判断一个元素是否出现在集合里**

通过key的索引，时间负责度 O（1）就可实现，无需遍历

## 哈希函数 Hash function

例如 将学生姓名映射到哈希表上为索引，通过hashCode将姓名转为数值，这样就可以映射到哈希表上做索引数字了。

> hashCode可以将不同数据类型通过特定编码方式，将其转化为数值

```cpp 
hashFunction = hashCode(name) % tableSize 
```

1. 如果hashCode得到的数值大于tablesize?
   为了保证映射的数值都落在哈希表上，会对数值进行取模的操作
2. 如果 数据量大于哈希表的大小怎么办？
   这时，多个数据映射到同一个索引，叫做**哈希碰撞**

## 哈希碰撞

有两种解决办法：**拉链法、线性探测法**

### 拉链法

数组和链表配合，冲突的元素存储来链表中，其实还是选择合适大小的哈希表，这样不会因为数组空值**浪费内存**，也不会因为链表太长**浪费查找时间**

### 线性探测法

- 一定要保证tableSize大于dataSize，利用数组空位解决碰撞问题



## 常见三种哈希结构

- 数组
- set（集合）
- map（映射）

| 集合        |  底层    |是否有序  |数值是否可重复  | 能否更改数值 | 查询效率 |增删效率|
| ----------- | ---- | ---- | ---- | ---- | ---|---|
| set         |红黑树   |  有序    |  不可重复    |   不可更改  | O(log n) |O(log n)|
| multiset    |红黑树  |    有序  | 可重复  |     不可更改 |O(log n)|O(log n)|
| unordered_set | 哈希表  |  无序  |      不可重复|    不可更改  |0(1)| O(1) |

红黑树是一种**平衡二叉搜索树**，所以key**是有序的**，但key**不可修改**，改动key的值会导致整棵树错乱，**所以只能增删**

| 映射  |  底层    |是否有序  |数值是否可重复  | 能否更改数值 | 查询效率 |增删效率|
| ----------- | ---- | ---- | ---- | ---- | ---|---|
| map      |红黑树   |  有序    |  不可重复    | key值不可更改 | O(log n) |O(log n)|
| multimap |红黑树  |    有序  | 可重复  | key值不可更改 |O(log n)|O(log n)|
| unordered_map | 哈希表  |   无序  |      不可重复| key值不可更改  |0(1)| O(1) |

##  总结

**当我们遇到要快速判断元素是否出现在集合里时，优先使用哈希法**

**牺牲空间换时间**





## [242. 有效的字母异位词 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-anagram/submissions/476567176/)

用哈希，把字母的相对位置作为key，出现次数作为value，

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        vector<int> s_vec(26,0);
        for (char a : s) {
            int index = (int) (a - 'a');
            s_vec[index]++;
        }
        for (char a : t) {
            int index = a - 'a';
            s_vec[index]--;
            
        }
        for (int i = 0; i < 26; i++) {
            if (s_vec[i] != 0) return false;
        }
        return true;

    }
};
```



## [349. 两个数组的交集 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-arrays/submissions/476630154/)

使用set（集合）：

将nums1拷贝到无序集合中，则set_nums为nums1的无重复数的版本，遍历nums2并调用find内置函数，如果在set_nums中找得到，说明此元素共有，写入result集合中（无重复）

- 注意返回值是vector<int> ,所以结果隐式的创建一个整型容器。

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set_nums(nums1.begin(),nums1.end());
        unordered_set<int> result;
        for (int num : nums2) {
            if (set_nums.find(num) != set_nums.end()) {
                result.insert(num);
            }
        }

        return vector<int>( result.begin(), result.end());
    }
};
```



## [202. 快乐数 - 力扣（LeetCode）](https://leetcode.cn/problems/happy-number/)



解题的关键在于：

- 如果不是快乐数，将无限循环，**sum将重复出现** 
  因为，如果sum还没重复过，说明目前依然能得到新的结果，所以运算还未完成，只有当结果为1，或者出现重复的结果了，才说明计算结束。
  还有一个问题，我不是数学系的，有没有可能他在正整数集中无限不循环？哈哈，已知他不是单调的
- getsum函数的写法，灵活使用取余和求余
- set内置函数find的使用
  如果找到指定元素，会返回元素所在位置的迭代器，如果找到尾部都没有的话，就返回end()
- 在主循环中别忘了用sum更新n的值

```cpp
class Solution {
public:
    int getSum(int n) {
        int sum = 0;
        while (n) {
            sum += (n % 10) * (n % 10);
            n /= 10;
        }
        return sum;
    }
    bool isHappy(int n) {
        unordered_set<int> sum_set;
        int sum = 0;
        while (1) {
            sum = getSum(n) ;
            if (sum == 1) return true;
            if (sum_set.find(sum) != sum_set.end()) {
                return false;
            }
            else {
                sum_set.insert(sum);
            }
            n = sum;
        }

    }
};
```



## [1. 两数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/two-sum/)

1. 为什么用哈希表

   查询一个元素是否出现过，虽然target并未直接可以查询的元素，但是它是由其他元素运算得到的，将运算的结果存入一个集合中的话，还是看这个target是否出现过。

2. 为什么用unordered_map
   不仅要知道元素有没有遍历过，还要知道元素的下标（总之就是有两个属性），使用key存元素，value存元素的下标,map正合适，又因为并不需要有序，也没有重复的元素，所以是unordered_map

3. 本题map是用来存什么的


   遍历nums中的元素，此元素之前的元素都被插入map中，在map中查找能与之匹配的元素，如果找不到，就把当前元素也插进map

4. map中的key和value用来存什么的
   map中的存储结构为 {key：数据元素，value：数组元素对应的下标}。

知识点：

- 插入map的元素为pair<int,int>
- auto iter 的类型为 unordered_map<int, int>::iterator
- 返回值为空容器{}，而不是空指针NULL



```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> num_map;
        for (int i = 0; i < nums.size(); i++) {
            auto  iter= num_map.find(target - nums[i]);
            if (iter != num_map.end()) {
                return {iter->second, i};
            }
            else {
                num_map.insert(pair<int,int> (nums[i], i));
            }
        }
        return {};

    }
};
```









