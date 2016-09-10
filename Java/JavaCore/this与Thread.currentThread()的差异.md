# this与Thread.currentThread()的差异
最近在看《Java多线程编程核心技术》时，对P20有一段关于this和Thread.currentThread()的输出结果有些疑惑<br/>
书上代码如下：
```
public class MyTest {
    public static void main(String[] args) {
        CountOperate countOperate = new CountOperate();
        Thread t = new Thread(countOperate);
        t.setName("A");
        t.start();
    }
}
class CountOperate extends Thread {
    public CountOperate() {
        System.out.println("CountOperate---begin");
        System.out.println("Thread.currentThread().getName()=" + Thread.currentThread().getName());
        System.out.println("this.getName()=" + this.getName());
        System.out.println("CountOperate---end");
    }
    @Override
    public void run() {
        System.out.println("run---begin");
        System.out.println("Thread.currentThread().getName()=" + Thread.currentThread().getName());
        System.out.println("this.getName()=" + this.getName());
        System.out.println("run---end");
    }
}
执行结果：
CountOperate---begin
Thread.currentThread().getName()=main
this.getName()=Thread-0
CountOperate---end
run---begin
Thread.currentThread().getName()=A
this.getName()=Thread-0
run---end
```
**以上代码创建线程的方式是向Thread传入Runnable的实现类Thread。** <br/>
首先需要强调的是: <br/>
- this代表当前类的对象
- Thread.currentThread()返回正在被哪个线程调用

**上面代码中共有两个线程对象countOperate和t，只不过countOperate被作为Runnable用于创建了线程t，setName("A")是线程t调用的，所以t的名字是t，并没有对countOperate设置名称，所以使用默认名称Thread-0。**

### 将代码更改如下：
```
public class MyTest {
    public static void main(String[] args) {
        CountOperate countOperate = new CountOperate("A");
        Thread t = new Thread(countOperate);
        t.start();
    }
}
class CountOperate extends Thread {
    public CountOperate(String name) {
        super(name);
        System.out.println("CountOperate---begin");
        System.out.println("Thread.currentThread().getName()=" + Thread.currentThread().getName());
        System.out.println("this.getName()=" + this.getName());
        System.out.println("CountOperate---end");
    }
    @Override
    public void run() {
        System.out.println("run---begin");
        System.out.println("Thread.currentThread().getName()=" + Thread.currentThread().getName());
        System.out.println("this.getName()=" + this.getName());
        System.out.println("run---end");
    }
}
执行结果：
CountOperate---begin
Thread.currentThread().getName()=main
this.getName()=A
CountOperate---end
run---begin
Thread.currentThread().getName()=Thread-0
this.getName()=A
run---end
```
**上面的代码是给countOperate线程设置名称为A，t线程使用默认名称。**