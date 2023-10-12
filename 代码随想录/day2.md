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

