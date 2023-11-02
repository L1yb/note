# size_type和size_t和int的区别

## size_type:

​		STL容器中的成员变量，用以保存不同容器的大小，被定义为unsigned 类型，具体大小取决于容器和机器，例如**string::size_type** 代表s**tring抽象长度的无符号整型**，从本质上来说是一个整数。这里的**抽象指的是机器的环境**，也就是说电脑不同，size_type可能不同。

## size_t:

​	定义在C/C++标准stddef.h中。表示对象的大小。size_t的真实类型和操作系统有关。

在32位机器中被普遍定义为：

```cpp
typedef unsigned int size_t
```

在64位机器中被普遍定义为：

```cpp
typedef unsigned long size_t
```

**在我的电脑上被定义为：

```cpp
#define __int64 long long 
typedef unsigned __int64 size_t
```

|名称|长度|具体类型|根据|
|---|---|---|---|
|int| 4byte|int|固定大小|
|size_t|目标平台下最大可能的数组尺寸|unsigned int(32)<br />unsigned long(64)|根据机器操作系统|
|size_type||unsigned <T>|根据容器类型和机器|



容易被坑的一些地方：

0x0001:

```cpp
string str="123";
int a=2;
cout<<a-str.size()<<endl;这段代码的输出并不是-1，在我64位win10系统中的输出是：4294967295，这是因为str.size()返回值是size_type一个无符号整数，而编译器在int与size_type做减法时，都视为了无符号整数。
```


0x0002:

我们在使用 string::find的函数的时候，它返回的类型就是 string::size_type类型。而当find找不到所要找的字符的时候，它返回的是 npos的值，这个值是与size_type相关的。假如，你是用 string s; int rc = s.find(.....); 然后判断，if ( rc == string::npos ) 这样在不同的机器平台上表现就不一样了。如果，你的平台的string::size_type的长度正好和int相匹配，那么这个判断会侥幸正确。但换成另外的平台，有可能 string::size_type的类型是64位长度的，那么判断就完全不正确了。 所以，正确的应该是： string::size_type rc = s.find(.....); 这个时候使用 if ( rc == string::npos )就回正确了
————————————————
版权声明：本文为CSDN博主「xushenbao_123」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_33688997/article/details/81449337
