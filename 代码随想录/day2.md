## 代码随想录训练营第2天|977.有序数组的平方、209.长度最小的子数组、59.螺旋数组Ⅱ

### [977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

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



### [209.长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

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



### [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/description/)

牛！

分析：二分法中提过，但是没有注意一个概念：**循环不变量，区间的定义就是不变量**

在刻画图形的时候要注意：

- 循环的次数
- 从左到右填充上
- 从上到下填充右
- 从右到左填充下
- 从下到上填充左

还有就是一定要**确保每个区间的开闭一致**，否则的话会**导致溢出**

关于为什么要区分奇偶：如果是奇数，没必要进行四次填充

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> result(n, vector<int>(n,0));
        int startX = 0,startY = 0;
        int loop = n / 2;//一圈一圈赋值
        int mid = n / 2;//中间位置，如果n为奇数才用得到
        int count = 1;//用来拷贝的初始值，递增到n*n
        int offset = 1;//循环结束一次，遍历的长度就缩短1
        int i,j;
        while (loop--) {
            //上
            for (j = startY; j < n - offset; j++ ) {
                result[startX][j] = count++;
            }
            //右
            for (i = startX; i < n - offset; i++) {
                result[i][j] = count++;
            }
            //下
            for (;j > startY; j--) {
                result[i][j] = count++;
            }
            //左
            for (; i > startX; i--) {
                result[i][j] = count++;
            }
            //起始位置+1
            startX++;
            startY++;
            //缩进+1
            offset++;

        }
        if (n % 2) result[mid][mid] = count;
        return result;

    }
};
```

- [54.螺旋矩阵(opens new window)](https://leetcode.cn/problems/spiral-matrix/)
- [剑指Offer 29.顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

### 数组总结

1. 数组是存放在连续内存空间上的相同数据类型的集合
2. 数组元素不能删除，只能覆盖（内存空间连续）.
3. vector的底层是array

#### 二分法

二分法时间复杂度：O（logn）

**循环不变量原则：区间的定义**

#### 双指针法

在一个for循环中实现两个for循环

#### 滑动窗口

本质还是双指针，根据当前子序列和大小，不断调节子序列的起始位置

#### 模拟行为

循环不变原则

