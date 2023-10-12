代码随想录day2

[977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

我的思路：数组本身是非递减的，所以，最大的元素在两端，但是在代码实现上思路不清晰，没想到如何写入新数组，尝试了一下在vector使用emplace、或者push_front，不行！下课回来再看

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

