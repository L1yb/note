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