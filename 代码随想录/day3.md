链表的定义

```cpp
//单链表
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x),next(NULL) {}
};
```



## [203. 移除链表元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-linked-list-elements/description/)

一、不设置虚拟头指针 AC

**在得到一个链表之后，通过定义一个链表类型的指针指向该链表的头指针，来进行操作**

- 链表指针的赋值实际上是重新定义指针的指向

- 需要临时指针来命名需要删除的指针，它本身没有命名，他的名字由上一个链表元素的next定位，当next需要改变指向时，将没有指针指向这块内存，届时将无法释放它。

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        
        while (head != nullptr && head->val == val) {
            ListNode* temp = head;
            head = head->next;
            delete temp;
        }
        ListNode* cur = head;
        while (cur != nullptr && cur->next != nullptr) {
            if (cur->next->val == val) {
                ListNode* temp = cur->next;
                cur->next = temp->next;
                delete temp;
            }
            else {
                cur = cur->next;
            }
        } 
        return head;
    }
};
```



二、虚拟头指针

只需要new一个新的链表元素指向头指针，使之不再是链表的头，那么就可以“一视同仁”

**注意在返回是dummyhead的next**，不能返回head、或者dummyhead，因为head有可能已经被删除，而dummyhead本不是链表的元素。

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        while (cur != nullptr && cur->next != nullptr) {
            if (cur->next->val == val) {
                ListNode* temp = cur->next;
                cur->next = temp->next;
                delete temp;
            }
            else {
                cur = cur->next;
            }
        }
        return dummyHead->next;
    }
};
```



## [707. 设计链表 - 力扣（LeetCode）](https://leetcode.cn/problems/design-linked-list/)

**单链表**

链表没有下标，所以这个自定义链表的下标是临时变量用来遍历的，不需要写入链表中，不同于数组，没有固定的下标的值。这个index就是遍历时的索引。

基本上每个成员函数都是三个步骤：

- 判断index合理性
- 索引到指定位置（之前）
- 增加或者删除

这里使用了虚拟头节点dummyhead，那么再索引的时候，避免了很多问题：

1. 头节点为空
2. 在头部插入
3. 定位到index前一位（现在直接从dummyhead开始，直接按照index索引，就能使用cur->next进行操作了）

> **delete** p       //p必须是指向动态分配内存的指针
>
> 释放指针后，并不会使指针的值变为null
>
> 使用new分配的空间，一定要用delete释放

```cpp
class MyLinkedList {
public:
    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int x) : val(x),next(nullptr){};
    };

    MyLinkedList() {
        this->dummyHead = new LinkedNode(0);
        this->size = 0;

    }
    
    int get(int index) {
        if (index >= size || index < 0) {
            return -1;
        }
        LinkedNode* cur = dummyHead->next;
        for (int i = 0; i < index; i++) {
            cur = cur->next;
        }
        return cur->val;  
    }
    
    void addAtHead(int val) {
        LinkedNode* temp = new LinkedNode(val); 
        temp->next = dummyHead->next;
        dummyHead->next = temp;
        size++;
    }
    
    void addAtTail(int val) {
        LinkedNode* temp = new LinkedNode(val);
        LinkedNode* cur = dummyHead;
        while (cur->next != nullptr) {
            cur = cur->next;
        }
        cur->next = temp;
        size++;
    }
    
    void addAtIndex(int index, int val) {
        // if (index == size) addAtTail(val);
        if (index > size) return;
        if (index < 0) index = 0;
        LinkedNode* cur = dummyHead;
        while (index--) {
            cur = cur->next;
        }
        LinkedNode* temp = new LinkedNode(val);
        temp->next = cur->next;
        cur->next = temp;
        size++;
    }
    
    void deleteAtIndex(int index) {
        if (index >= size || index < 0) return;
        LinkedNode* cur = dummyHead;
        while (index--) {
            cur = cur->next;
        }
        LinkedNode* temp = cur->next;
        cur->next = temp->next;
        delete temp;
        //delete命令指示释放了tmp指针原本所指的那部分内存，
        //被delete后的指针tmp的值（地址）并非就是NULL，而是随机值。也就是被delete后，
        //如果不再加上一句tmp=nullptr,tmp会成为乱指的野指针
        //如果之后的程序不小心使用了tmp，会指向难以预想的内存空间        
        temp = nullptr;
        size--;
    }
private:
    int size;
    LinkedNode* dummyHead;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```



## [206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/submissions/474304380/)

简单题，一点都不简单！

**双指针法：**

定义cur 、pre用来交换

定义temp用来保存cur->next	

pre初始化为空

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = head;
        ListNode* pre = nullptr;
        ListNode* temp;
        while (cur != nullptr) { 
            temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
};
```



**递归法：**

相当于替换了cur和pre的赋值，用递归替代while循环

```cpp
class Solution {
public:
    ListNode* reverse(ListNode* pre, ListNode* cur) {
        if (cur == nullptr) return pre;
        ListNode* temp = cur->next;
        cur->next = pre;
        return reverse(cur, temp);
    }

    ListNode* reverseList(ListNode* head) {        
        return reverse(nullptr,head);
    }
};
```













