# Java多线程
**概念：线程可以理解成是在进程中独立运行的子任务（使用多线程就是在使用异步）。**
### 创建线程
- 继承Thread类（Thread类已实现了Runnable接口）
- 实现Runnable接口

### 启动线程
手动调用start()方法后，系统会安排一个时间来调用Thread中的run()方法，之后线程便开始运行。 <br/>
- 执行start()方法的顺序不代表线程启动的顺序
- 如果直接调用run()方法，则由主线程（main）执行run()方法内的代码

## 线程常用的方法
### currentThread()方法
**currentThread()方法可返回代码正在被哪个线程调用的信息。**
- this.getName()与Thread.currentThread().getName()
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
为什么上面程序的两个this.getName()结果都是Thread-0呢？ <br/>
