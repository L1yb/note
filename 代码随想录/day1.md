## 代码随想录训练营第一天|704.二分查找、27.移除元素

### [LeetCode 704 二分查找](https://leetcode.cn/problems/binary-search/description/)

二分法的前提： **有序数组，数组无重复元素**.

**补充：循环不变量：区间的定义**

**tips：** 

1. 我对于开闭区间的理解是，重置边界时借助middle，因为**middle是已经验证过不符合要求的**，如果直接利用middle赋值，说明这个边界**不会再取到**，也就是**开区间**
2. 可以通过先**确定循环条件**  来 **确定区间**，反之亦然
3. 第一种方法因为**左右边界都能取到**，所以区间处理上，middle没必要再取了，并且right必须有效，right = size -1
4. 第二种方法**左边界可以取到**，所以左边界没必要再取middle，右边界可以取到数组范围之外，right = size

第一种写法：闭区间

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        while (left <= right) {
            int middle = left + (right - left) / 2;
            if (nums[middle] > target) {
                right = middle - 1;
            }else if (nums[middle] < target) {
                left = middle + 1;
            }else return middle;
        }
        return -1;
    }
};
```

第二种写法：[L, R)

``` cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        //[L, R)
        while (left < right) {
            int middle = left + (right - left) / 2;
            if (nums[middle] > target) {
                right = middle;
            }else if (nums[middle] < target) {
                left = middle + 1;
            }else return middle;
        }
        return -1;
    }
};
```

***总结： 先确定用什么边界，再确定循环条件，对应方法记住套路。***



### [LeetCode 27 移除元素](https://leetcode.cn/problems/remove-element/description/)

方法一：自己的方法

因为是原地修改，并且不考虑超出新长度外的元素，所以我的第一想法是：将找到的元素和最后面的非val值的元素交换位置，然后长度减一

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            if (nums[i] == val) {
                while (nums[size - 1] == val && size - 1 > i) {
                    --size;
                }//结尾是val，直接忽视，size -1
                nums[i] = nums[size - 1]; //结尾不是val，拷贝到前面去，size - 1
                size--;                
            }
        }
        return size;
    }
};
```



方法二： **双指针（快慢指针法）**：通过一个快指针和一个慢指针**在一个for循环中完成两个for循环的工作**

快指针：寻找新数组的元素，也就是不含val的元素数组（不会停）

慢指针：更新新数组的下标（只要不等于val，就更新数组下标，等于val时，slowIndex等待fastIndex去找新元素，来替代val）

>  个人理解：
>
> 相当于两个数组，fast下标遍历原数组，找到之后通过slow下标拷贝到新数组
>
> 之所以能够在一个数组中操作，是因为fast和slow重合时意味着两个数组相同，不重合时，数组大小由slow定义



[27.移除元素-双指针法.gif (498×346) (bcebos.com)](https://code-thinking.cdn.bcebos.com/gifs/27.移除元素-双指针法.gif)



```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.size();fastIndex++) {
            if (nums[fastIndex] != val) {
                nums[slowIndex++] = nums[fastIndex];
            }
        }
        return slowIndex;

    }
};
```



