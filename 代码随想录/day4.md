## [24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/)

一开始思路是 1指向3，2指向1————2->1->3

但是到了第二轮就成了3->5,4->3

此时1也指向3，所以这个方法不行。不仅是交换顺序，而是cur的位置也不对。

正确思路是这两个图片，cur= dummyhead，后续cur都位于需要交换的两个元素之前的位置，这样才能让交换之后的结果接的上前面。

[代码随想录 ](https://programmercarl.com/0024.两两交换链表中的节点.html#算法公开课)

[图一](https://code-thinking.cdn.bcebos.com/pics/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B92.png)

[图二](https://code-thinking.cdn.bcebos.com/pics/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B93.png)


```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == nullptr || head->next == nullptr) return head;
        ListNode* dummyhead = new ListNode();
        dummyhead->next = head;
        ListNode* cur = dummyhead;
        while (cur->next != nullptr && cur->next->next != nullptr) {
            ListNode* temp1 = cur->next;
            ListNode* temp2 = cur->next->next->next;
            cur->next = cur->next->next;
            cur->next->next = temp1;
            temp1->next = temp2;
            cur = cur->next->next;
        }
        return dummyhead->next;

    }
};
```

## [19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

**暴力解法：先遍历获取链表长度，再索引到指定位置。**

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int size = 0;
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* cur = dummyHead->next;
        while (cur) {
            cur = cur->next;
            size++;
        }
        int index = size - n;//删除元素的上一位
        cur = dummyHead;
        while (index--) {
            cur = cur->next;
        }
        ListNode* delNode = cur->next;
        cur->next = delNode->next;
        delete  delNode;
        return dummyHead->next;

    }
};
```

**双指针法：**快慢指针

快指针索引到链表的结尾

慢指针定位到删除元素的上一位

中间的距离为n + 1，定义一个计数器。

快指针走了n+1步之后，慢指针跟着移动

> 本题不需要检查n的合理性，题目已知

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* fast = dummyHead;
        ListNode* slow = dummyHead;
        int cnt = 0;
        while (fast->next != nullptr) {
            fast = fast->next;
            cnt++;
            if (cnt >= (n + 1)) {
                slow = slow->next;
            }
        }
        ListNode* temp = slow->next;
        slow->next = temp->next; 
        delete temp;
        return dummyHead->next;
    }
};
```

## [面试题 02.07. 链表相交 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

不会

方法是：尾对齐，从较短的链表对应位置开始

- 先得到两个链表的长度

- 计算差值def（让curA指向较长的链表）（降低代码重复率）
- curA移动def次
- 两个指针同时移动
- 比较的是节点是否相等，而不是节点的值

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == NULL || headB == NULL) return NULL;
        int sizeA = 1;
        int sizeB = 1;
        ListNode* curA = headA;
        ListNode* curB = headB;
        while (curA) {
            curA = curA->next;
            sizeA++;
        }
        while (curB) {
            curB = curB->next;
            sizeB++;
        }
        curA = headA;
        curB = headB;
        ;;;;;;;
        if (sizeA < sizeB) {
            swap(sizeA, sizeB);
            swap(curA,curB);
        } //使得curA指向较长的链表
        int def = sizeA - sizeB;
        while (def--) {
            curA = curA->next;
        }
        while (sizeB--) {
            if (curA == curB) {
                return curA;
            }
            curA = curA->next;
            curB = curB->next;
        }
        ;;;;;;;

        // int def = sizeA - sizeB;
        // if (def >= 0) {
        //     while (def--) {
        //         curA = curA->next;
        //     }
        //     while (sizeB--) {
        //         if (curA == curB) {
        //             return curA;
        //         }
        //         curA = curA->next;
        //         curB = curB->next;
        //     } 
        // }
        // if (def < 0) {
        //     def = -def;
        //     while (def--) {
        //         curB = curB->next;
        //     }
        //     while (sizeA--) {
        //         if (curA == curB) {
        //             return curA;
        //         }
        //         curA = curA->next;
        //         curB = curB->next;
        //     } 
        // }
        return NULL;


    }
};
```

## [142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/)

> 2023/10/22 哎，太摆烂了，一连一周，拿上课、约会、比赛等等做借口，睡懒觉、熬夜、玩手机，第四天就没坚持住了，补补叭

**双指**：知道是双指针，快慢指针，但是具体操作想不到

定义快指针每步走两次，慢指针每步一次，如果有环，必然会相遇，得到一个相遇点，从相遇点到入口的长度 等于 从head到入口的长度（数学推导）

[推导过程](https://programmercarl.com/0142.环形链表II.html#思路)

其中关键的点：

- **fast 的路程 = 2 slow**

- ，**从相遇点到入口的长度 等于 从head到入口的长度**，无论是第几圈相遇，不同的是经过的圈数  k ( y + z)
- **slow的路程一定是x + y** ,不会绕一圈以上
  因为 假如 fast 和slow同时 从环的入口出发，那么必然在fast绕两圈，slow绕一圈回到入口相遇。现在，slow到达入口时，fast必然已经在环内，fast slow距离为k，一圈的长度为n，（k + n）/ 2   < n,说明 slow并未绕完一周，就已经相遇
- **fast无法跳过slow**，fast步长为2，slow步长为1 ， fast相对于slow步长为1，fast一步一步逼近slow，不会跳过去



```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        
        while (fast != NULL && fast->next != NULL) {
            fast = fast->next;
            fast = fast->next;
            slow = slow->next;
            //找到相遇的点
            if (fast == slow) {
                ListNode* index1 = head;
                ListNode* index2 = fast;
                //通过数学分析，从相遇的点到入口的距离  等于 从head到入口的距离
                while (index1 != index2) {
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index1;
            }

        }
        return NULL;
        
    }
};
```

