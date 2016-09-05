# Java I/O体系
## I/O体系图
![](http://i.imgur.com/HjlBUF2.jpg)

## I/O类库概述
### File类
**File类主要是完成了文件夹管理的命名、查询文件属性和处理目录等到操作它不进行文件夹内容的读取操作。**
- File既可以代表一个文件，也可以代表一个文件集（目录）
- 如果代表文件集，可以使用list()方法获取其下文件或目录
- File也可以代表新的文件或目录
- File直接处理的是文件和文件系统，它描述的是文件本身的属性，并非流式操作

### 输入和输出
**I/O类库常使用流这个抽象概念，它代表任何有能力产生数据的数据源对象或者是有能力接收数据的接收端对象。**
#### InputStream类型
InputStream的作用是用来表示那些从不同数据源产生输入的类。<br/>
数据源包括： <br/>
> 字节数组 如：ByteArrayInputStream <br/>
> String对象 如：StringReader <br/>
> 文件 <br/>
> "管道" <br/>
> 多种到合并到一个流内 <br/>
> 网络数据流 <br/>

- **ByteArrayInputStream** <br/>
允许将内存的缓冲区当作InputStream使用，参数为字节数组，直接将参数的内容作为产生的数据。
- **FileInputStream** <br/>
用于从文件中读取信息
- **PipedInputStream** <br/>
用于读取另一个线程发送的数据。<br/>
*在java中，PipedOutputStream和PipedInputStream分别是管道输出流和管道输入流。
它们的作用是让多线程可以通过管道进行线程间的通讯。在使用管道通信时，必须将PipedOutputStream和PipedInputStream配套使用。
使用管道通信时，大致的流程是：我们在线程A中向PipedOutputStream中写入数据，这些数据会自动的发送到与PipedOutputStream对应的PipedInputStream中，进而存储在PipedInputStream的缓冲中；此时，线程B通过读取PipedInputStream中的数据。就可以实现，线程A和线程B的通信。* [使用案例](http://blog.csdn.net/hzw2312/article/details/6395778)
- **SequenceInputStream** <br/>
将两个或多个InputStream对象转换成单一InputStream，从第一个输入流开始读取，直到到达文件的末尾，接着从第二个输入流读取，依次类推，直到到达包含的最后一个输入流的文件的末尾为止。

#### OutputStream类型
该类别决定了输出所要去往的目标：字节数组、文件或管道
- **ByteArrayOutputStream** <br/>
在内存中创建缓冲区，所有送往"流"的数据都要放置在此缓冲区。
- **FileOutputStream** <br/>
用于将数据写入文件。
- **PipeOutputStream** <br/>
向其它线程发送数据。

#### FilterInputStream与FilterOutputStream类型
- **DataInputStream/DataOutputStream** <br/>
读写基本类型数据 （int,char,long等），读写顺序要一致，比如连续写入两个int，就需要连续读取两个int。
- **BufferedInputStream/BufferedOutputStream** <br/>
使用缓冲区，防止频繁地进行实际读写操作。
- **ObjectInputStream/ObjectOutputStream** <br/>
对象与流之间的读写，只能将支持java.io.Serializable接口的对象写入流中。

