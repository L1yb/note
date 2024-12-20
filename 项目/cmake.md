

简单编译文件

```bash	
g++ -o 输出文件名 源文件名.cpp
//其中，`-o` 参数用于指定输出文件的名称
```

```bash
g++ -o my_program main.cpp
//这将编译main.cpp并生成一个名为my_program的可执行文件。

//运行可执行文件：一旦编译成功，您可以运行生成的可执行文件。在终端中，只需键入可执行文件的名称并按回车键：
./my_program


```

[windows 10上使用vscode编译运行和调试C/C++ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/77645306)





# CMakeLists

一、项目文件结构

>> .vscode
>> >c_cpp_properties.json
>> >
>> >tasks.json
>> >
>> >launch.json//与CMakeList无关
>>
>> build 
>>
>> > 主要用来存放生成的Makefile以及可执行文件
>>
>> include
>>
>> > 主要存放我们自己编写的头文件
>>
>> src
>>
>> > 存放自己编写的cpp文件
>>
>> bin
>>
>> > 存放动态链接库
>>
>> lib
>>
>> > 存放静态链接库

二、 各个文件的作用

2.1 .vscode

一般包含三个配置文件：

>c_cpp_properties.json
>
>tasks.json
>
>launch.json    //与CMakeList无关

1. c_cpp_properties.json
   	c_cpp_properties.json是用于**配置编译器环境**的，最重要的是**includePath**,其**指明了C/C++标准库、用户头文件所在位置**。如果我们使用的库不在includePath中，需要手动添加。这样的话**不需要CMakeLists也可以编写C/C++**。

```json
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**",
                "/usr/include/**",
                "/usr/local/**",
                "/usr/lib/**"
            ],//头文件路径
            "defines": [],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}
```

2. tasks.json

   ​	 tasks.json的主要作用的添加构建（编译、链接等）任务。也就是说，我们除了淡出编辑源码外，想要编译、链接生成可执行文件，必须配置这个文件。当然，**CMakeLists可以替代这个文件**。

创建tasks.json的文件的方法如下：

- 首先，按住ctrl+shift+p打开命令行，输入

  ```json
  Tasks: Run task
  ```

  会自动生成一个json文件

3. launch.json
      launch.jso主要是调试时的配置文件，也就是说，如果我们单纯只是向生成可执行文件，其实这个文件也可以没有





