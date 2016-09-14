# Java NIO
## Channel
**通道的特点：**<br/>
- 既可以从通道中读取数据，又可以写数据到通道；但流的读写是单向的。
- 通道可以异步读写。
- 通道中的数据总是要先读到一个Buffer，或者是要先写入一个Buffer。

**重要的通道：**<br/>
- FileChannel <br/>
从文件中读写数据。
- DatagramChannel <br/>
能通过UDP读写网络中的数据。
- SocketChannel <br/>
能通过TCP读写网络中的数据。
- ServerSocketChannel <br/>
监听新的TCP连接，为每个连接创建SocketChannel。

## Buffer
**Java NIO中Buffer（缓冲区）用于和NIO通道进行交互；缓冲区本质上是一块可以写入数据，然后可以从中读取数据的内存，这块内存被包装成NIO Buffer对象，并提供了一组方法，用于访问该块内存。** <br/>

![](http://i.imgur.com/gryBE98.png)

使用Buffer读写数据一般遵循以下四个步骤：
- 写入数据到Buffer（如果使用put写入，putXX与getXX应该对应）
- 调用flip()方法（切换读写模式，将position置为0）
- 从Buffer中读取数据
- 调用clear()方法或者compact()方法

Buffer的三个属性：
- capacity <br/>
Buffer的固定大小。
- position <br/>
写入数据时，position表示当前的位置，初始值为0，每写入一个数据position就向前移动到下一个可插入数据的Buffer单元（所以在写数据的过程中position直接指向的是没有数据空单元），position最大可为capacity-1。<br/>
读取数据时，当将Buffer从写模式切换到读模式，position会被重置为0，当从Buffer的position处读取数据时，position向前移动到下一个可读的位置。
- limit <br/>
在写模式下，Buffer的limit表示你最多能往Buffer里写多少数据。<br/>
在读模式下，limit表示最多能读到多少数据。

## Scatter/Gather
- **Scatter（分散）从Channel中读取是指在读操作时将读取的数据写入多个Buffer中。**
- **Gather（聚集）写入Channel是指在写操作时将多个Buffer的数据写入同一个Channel。**
```
public class StreamTest {
    public static void main(String[] args) throws IOException {
        ByteBuffer header = ByteBuffer.allocate(6);
        ByteBuffer body = ByteBuffer.allocate(4);
        ByteBuffer[] buffers = {header, body};
        header.put("header".getBytes());
        body.put("body".getBytes());
        RandomAccessFile file = new RandomAccessFile(new File("F:/test.txt"), "rw");
        FileChannel channel = file.getChannel();
        header.flip();
        body.flip();
        channel.write(buffers);
    }
}
```
## FileChannel
**连接文件的通道，无法设置为非阻塞模式，总是运行在阻塞模式下。**
- 打开FileChannel <br/>
需要通过一个InputStream、OutputStream或RandomAccessFile来获取一个FileChannel实例。
- 将FileChannel中的数据读取到Buffer中。
- 用完FileChannel必须关闭，channel.close()。

## ServerSocketChannel与SocketChannel
**类似于ServerSocket与Socket，基于TCP。** <br/>
服务端：
```
public class Launcher {
    public static void main(String args[]) throws IOException {
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        serverSocketChannel.socket().bind(new InetSocketAddress(20000));
        serverSocketChannel.configureBlocking(false);
        while (true) {
            SocketChannel socketChannel = serverSocketChannel.accept();
            if (socketChannel != null) {
                System.out.println("连接成功");
                new Thread(new ServerThread(socketChannel)).start();
            }
        }
    }
}
class ServerThread implements Runnable {

    private SocketChannel socketChannel;

    public ServerThread(SocketChannel socketChannel) {
        this.socketChannel = socketChannel;
    }

    @Override
    public void run() {
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        try {
            int size = socketChannel.read(buffer);
			//切换为读模式
            buffer.flip();
            while (buffer.hasRemaining()) {
                System.out.print((char) buffer.get());
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                socketChannel.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```
客户端：
```
public class Launcher {
    public static void main(String[] args) throws IOException {
        SocketChannel socketChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 20000));
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        buffer.put("hello".getBytes());
		//切换为写模式
        buffer.flip();
        while (buffer.hasRemaining()) {
            socketChannel.write(buffer);
        }
    }
}
```
## DatagramChannel
**类似于DatagramSocket，基于UDP。** <br/>
服务端：
```
public class Launcher {
    public static void main(String args[]) throws IOException {
        DatagramChannel datagramChannel = DatagramChannel.open();
        datagramChannel.bind(new InetSocketAddress(20000));
        ByteBuffer buf = ByteBuffer.allocate(1024);
        buf.clear();
        datagramChannel.receive(buf);
        //切换为读模式
        buf.flip();
        while (buf.hasRemaining()) {
            System.out.print((char) buf.get());
        }
    }
}
```
客户端：
```public class Launcher {
    public static void main(String[] args) throws IOException {
        DatagramChannel datagramChannel = DatagramChannel.open();
        ByteBuffer buf = ByteBuffer.allocate(1024);
        buf.clear();
        buf.put("hello".getBytes());
        //切换为读模式（需要将buf的数据写入datagramChannel）
        buf.flip();
        datagramChannel.send(buf, new InetSocketAddress("127.0.0.1", 20000));
    }
}

```
## 通道之间的数据传输
如果两个通道中有一个是FileChannel，可以直接将数据从一个Channel传输到另一个Channel。

- **transferFrom()** <br/>
将数据从源通道传输到FileChannel中。
```
public class StreamTest {
    public static void main(String[] args) throws IOException {
        RandomAccessFile fromFile = new RandomAccessFile("F:/from.txt", "rw");
        FileChannel fromChannel = fromFile.getChannel();
        RandomAccessFile toFile = new RandomAccessFile("F:/to.txt", "rw");
        FileChannel toChannel = toFile.getChannel();
        toChannel.transferFrom(fromChannel, 0, fromChannel.size());
    }
}
```
- **transferTo()** <br/>
将数据从FileChannel传输到其他的Channel中。
```
public class StreamTest {
    public static void main(String[] args) throws IOException {
        RandomAccessFile fromFile = new RandomAccessFile("F:/from.txt", "rw");
        FileChannel fromChannel = fromFile.getChannel();
        RandomAccessFile toFile = new RandomAccessFile("F:/to.txt", "rw");
        FileChannel toChannel = toFile.getChannel();
        fromChannel.transferTo(0, fromChannel.size(), toChannel);
    }
}
```
## 示例代码
读取磁盘文件
```
public class StreamTest {
    public static void main(String[] args) throws IOException {
        RandomAccessFile file = new RandomAccessFile(new File("F:/hello.txt"), "rw");
        FileChannel channel = file.getChannel();
        ByteBuffer buf = ByteBuffer.allocate(48);
        channel.read(buf);
        //切换为读模式
        buf.flip();
        while (buf.hasRemaining()) {
            System.out.print((char) buf.get());
        }
        buf.clear();
        file.close();
    }
}
```
写入磁盘文件
```
public class StreamTest {

    public static void main(String[] args) throws IOException {
        RandomAccessFile file = new RandomAccessFile(new File("F:/hello.txt"), "rw");
        FileChannel channel = file.getChannel();
        ByteBuffer buf = ByteBuffer.allocate(48);
        buf.put("Java".getBytes());
        //切换为读模式
        buf.flip();
        channel.write(buf);
        buf.clear();
        file.close();
    }
}
```



 
