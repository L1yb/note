[安装ASIO](https://sourceforge.net/projects/asio/files/asio/1.28.0%20%28Stable%29/)库到SDK

属性->VC++目录->外部包含目录->asio/include

> address:
>
> 93.184.216.34
>
> 51.38.81.49 		//size相当大



## 同步模式

直接运行没有任何数据，添加了这一步

头文件 `#include <chrono>`

		//当服务器距离很远，全速运行时可能没有时间进行操作
		using namespace std::chrono_literals;
		std::this_thread::sleep_for(1000ms);

two tips

- 不知道事件何时发生，一旦离开了我们的计算机，就超出了我们的控制范围，这种情况需要时间
- 我们不知道服务器需要提前准备多少数据、应该将缓冲区设置为多大

添加阻塞事件非常的浪费！

------------------------------------------------------------

## 异步模式

利用asio的异步方面来做



![image-20240411222215260](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20240411222215260.png)





[scoped_lock](https://en.cppreference.com/w/cpp/thread/scoped_lock)

[std::move()]([C++性能优化利器：std::move() - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/256239799)) 将对象的状态或者所有权从一个对象转移到另一个对象，只是转移，没有内存的搬迁或者内存拷贝。

因此，通过std::move()，可以避免不必要的拷贝操作。
