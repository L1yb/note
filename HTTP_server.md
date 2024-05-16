# Building an HTTP Server From Scratch in C++

> HTTP代表超文本传输协议,是通过互联网发送和接收数据的标准。

HTTP Server是在网络上通过http进行数据服务的计算机，它是web server的基本组块

以浏览器为例

- 接收这个请求并处理它
- 在检查后确定适当的相应
- 发送响应到web客户端

通过http server发送的响应还可以包括来自web 服务器存储的数据



## high-level design

我们的服务器使用tcp/ip注册一个ip地址

需要一个特殊的端口，套接字将通过这个端口来侦听来自网络的连接

进入我们服务器的网络连接将被存储在网络线程队列中

当队列满了之后，传入的网络请求将会失败

服务器将会同步处理每个线程

网络连接将会被用于创建临时的套接字，服务器通过这个套接字接收来自客户端的数据，并向客户端发送数据

**总之，我们需要一个套接字**：

- 侦听网络连接并把他们送入队列
- 从队列中同时接收网络链接
- 通过网络连接读取客户端发来的数据（请求）
- 通过网络连接发送数据（响应）给客户端

## implementation 执行

windows中将使用“Winsock”库，linux将使用“Sys/socket" and "Arpa/inet"

实现上的差别在于两者系统可执行API的行为

## OOP design 面向对象设计

TcpServer类将是所有服务器代码的抽象层。

## starter files 启示文件











