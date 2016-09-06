# Java网络编程
## InetAddress类
**java.net.InetAddress类是Java对IP地址（包括IPv4和IPv6）的高层表示，它一般包括一个主机名和一个IP地址。**
- 如果用InetAddress.getByAddress(byte[] addr)创建InetAddress对象时，byte的表示范围是-128~127，当IP地址中的某个值超过范围后应当转为byte类型的，如：byte[] ip = {(byte) 220, (byte) 181, 111, (byte) 188}
- 调用getByName()并提供一个IP地址串作为参数时，会为所请求的IP地址创建一个InetAddress对象，而不检查DNS。
- 一个主机名（域名）可能对应多个地址，可以用InetAddress.getAllByName(String host)方法来获取

## ServerSocket
**服务端接收客户端Socket连接**
- ServerSocket使用其accept()方法监听这个端口的入站连接（会返回一个socket对象），阻塞式方法

## Socket（基于TCP协议）
**Socket连接是全双工的，两台主机都可以同时发送和接收数据**
- 当创建一个socket对象时，实际是在网络上建立连接
- 构造Socket时，端口号参数传0代表Java会随机选择1024到65535之间的一个可用端口
- 可以构造一个未连接的套接字，之后可用connect()方法连接
- 可以构造一个使用代理服务器的Socket
- 通过socket对象可以get输入或输出流，用于在网络上发送数据
- close()方法会同时关闭Socket输入和输出
- shutdownInput()和shutdownOutput()方法可以只关闭连接的一半，并不会关闭Socket，也不会释放与Socket关联的资源，所以用之后还需关闭Socket
- 如果直接关闭流，Socket也会被关闭
```
//Socket是否关闭
isClose()

//Socket是否连接过（并非指当前socket的连接状态）
isConnect()

//设置read()方法阻塞的最短时间，超时抛出异常后，无法读取到数据（实验测试），参数0表示不限超时
setSoTimeout(int milliseconds)

//true表示可确保包会尽可能块地发送，无论包的大小
setTcpNoDelay(boolean on)

//决定Socket关闭时如何处理尚未发送的数据报
setSoLinger(boolean on, int seconds)
```

## DatagramSocket（基于UDP协议）
**不需要建立连接，直接发送数据**