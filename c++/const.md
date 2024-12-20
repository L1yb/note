# const的使用

## const值

const指定变量

const在指针的声明中**从右往左读**

- **指向常量的指针point to cons**t，普通指针不能指向常量
  `const char * ptr`
- **const point常量指针**（不可更改指向，可以修改指向对象的值）
  `char * const ptr`

两个概念：

1. 顶层const（top-level const）：指针本身是常量
2. 底层const (low-level const)  ：指针所指对象是常量

## const成员函数

括号后添加const关键字将指定该函数为**只读函数**，不会修改其调用该函数的对象

## const 类

创建一个const的class对象，**只能调用const成员函数**，所以，在写成员函数时，**如果并不修改对象本身，最好在函数后面加const**，这样，无论创建对象时无论是否为const，都可以调用。**如果对象是指针，或者对象成员包含指针，能够修改取决于const的位置**。

const一个对象，**相当于将其所有的成员都变成了const成员**

- **不再能修改他的值**，但是能够**调用const成员函数**

- **对于指针成员，同样不能修改指向的内容，但是可以修改指针的指向**





## 理解const的方法：**从右往左读**

```cpp
int a = 1;
int * p0 = &a;
const int *p1 = &temp1; //p1是一个指针，指向int常量
int const *p2 = &temp2; //同p1
int * const p3 =  p0; //p3是一个常量对象，p3是一个常量指针，p3是指向int的常量指针
const int * const p4 = p1; //p4是一个常量指针，指向const int
```

括号后添加const关键字将指定该函数为**只读函数**，不会修改其调用该函数的对象
