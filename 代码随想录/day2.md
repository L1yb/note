代码随想录day2

[977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

我的思路：数**组本身是非递减的**，所以，**最大的元素在两端**，但是在代码实现上思路不清晰，没想到如何写入新数组，尝试了一下在vector使用emplace、或者push_front，不行！下课回来再看

本题还是用到了**双指针**的方法，但是目前来看双指针在数组中更多指的是双下标索引

对于vector的 操作还不熟练，对于新元素不知道怎么记录，最简单的就是创建一个容器并初始化分配空间，后面再拷贝就好了。

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int size = nums.size() - 1;
        vector<int> result(size+1,0);
        for (int i =0, j = size; i <= j; ) {
            if ( nums[i] * nums[i] > nums[j] * nums[j]) {
                result[size--] = nums[i] * nums[i];
                i++;
            }
            else {
                result[size--] = nums[j] * nums[j];
                j--;
            }

        }
        return result;
    }
};

```

补充：

vector不支持push_front\emplace_front

insert：有返回值，常用于在特定位置重复插入元素，**拷贝**

emplace：无返回值，在容器中**直接构造**元素，并非拷贝（但是这又意味着什么呢？）



[209.长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

**没做出来**

分析：本题的解题方法为**滑动窗口**，不断调整子序列的起始位置和终止位置。本质上还是**双指针**。

**适用情况是本身是两层for循环的化简**，**内循环次数受外循环限制，就像一个滑动的窗口**，现在化简内层循环

滑动窗口需要明确的是：

1. 窗口内是什么：
2. **如何移动窗口起始位置**
3. 如何移动窗口结束位置

窗口就是 满足sum>=target的连续子数组

起始位置当满足窗口要求时移动

结束位置负责遍历数组，是外层for循环的索引

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int sublength = 0;
        int sum = 0;
        int result = INT_MAX;
        int i = 0;
        for (int j = 0; j < nums.size(); j++) {
            sum += nums[j];
            while (sum >= target) {
                sublength = j - i + 1;
                result = result < sublength ? result : sublength;
                sum -= nums[i++];
            }
        }    
        return result == INT_MAX ? 0 : result;

    }
};
```

相关题目

- [904.水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)

- [76.最小覆盖子串(opens new window)](https://leetcode.cn/problems/minimum-window-substring/)
