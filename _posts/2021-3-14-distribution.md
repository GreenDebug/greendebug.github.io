# 概述

## 分布式系统定义

由多个通过**网络互联**的独立自治的**计算节点**组成。这些计算节点基于**消息传递机制**进行**互相协作**，已完成共同的目标

从用户来看，分布式系统是计算结点内聚在一起的一个整体 

计算节点：计算机、进程 、线程、虚拟机、有限状态机

独立自治：每个节点有自己的CPU、时钟

消息传递：并非内存共享模型

![image-20210314160223797](https://gitee.com/csjuesz/image/raw/master/20210314160230.png)

节点没有公共状态、必须通过消息写作。通信复杂度是影响效率重要因素。设计时必须应对局部失效、消息延迟/丢失错误

![image-20210314160405706](https://gitee.com/csjuesz/image/raw/master/20210314160405.png)

不同层次的并行运算

指令集并行、CPU多核并行、多CPU并行（一致性/非一致性）、GPU并行、多机并行（消息传递）

## 目的

1. 提高计算能力
2. 提高存储能力
3. 提高网络吞吐能力
4. 提高可靠性
5. 提高安全性
6. 提高可靠性
7. 实现资源共享
8. 实现跨越时空的协同服务

可扩展性（垂直、水平）

容错性

透明性

开发性

安全性

可维护性

## 挑战

异构性：软硬件差异

自治

局部视图

开放性：数目在变动，网络在变动

可扩展性

故障处理

安全性

透明性

服务质量保证

### 节点染色

```mermaid
graph LR;
	12-->33-->15-->20-->27-->37-->42-->13
```

相邻颜色不同

Repeat forever：

- Send message c to all neighbours

- Receive message from all neighbours. Let M be the set of message received.

- $If\ c \notin \{1,2,3\}\ \and\ c > max\ M: $

  $Let\ c \leftarrow min(\{1,2,3\}-M)$     

### 数据库备份

分布式一致性：Paxos协议、Raft协议



# Java  Socket

## 线程

线程：有独立上下文，独立CPU寄存器，共享地址，主函数结束生命结束

Java线程 Thread类重载run函数

```java
class MyThread extends Thread{
    //输出1-ticket
    private int ticket;

    MyThread(int t){
        ticket = t;
    }

    public void run(){
        for(int i=0; i < ticket; i++){
            System.out.println(this.getName()+" sold ticket"+ i);
        }
    }
}
```

```java
public class ThreadTest{
    //主函数
    public static void main(String[] args){
        MyThread t1 = new MyThread(10);
        MyThread t2 = new MyThread(15);
        MyThread t3 = new MyThread(30);
        t1.start();
        t2.start();
        t3.start();
    }
}
```

## Socket服务器

Java输入输出流

stream字节、reader/writer字符、buffer缓冲

![image-20210314213050690](https://gitee.com/csjuesz/image/raw/master/20210314213050.png)

```java
import java.io.*;
import java.net.*;

public class ServerThread extends Thread {
    Socket socket = null;

    public ServerThread(Socket socket) {
        this.socket = socket;
    }
	//读信息，打印到命令行，将信息发回去
    public void run() {
        InputStream is = null;
        InputStreamReader isr = null;
        BufferedReader br = null;
        OutputStream os = null;
        PrintWriter pw = null;
        try {
            is = socket.getInputStream();
            isr = new InputStreamReader(is);
            br = new BufferedReader(isr);
            os = socket.getOutputStream();
            pw = new PrintWriter(os);
            String info = null;
            while ((info = br.readLine()) != null) {
                System.out.println("Message from client:" + info);
                System.out.println(info);
                pw.flush();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (pw != null)
                    pw.close();
                if (os != null)
                    os.close();
                if (br != null)
                    br.close();
                if (isr != null)
                    isr.close();
                if (is != null)
                    is.close();
                if (socket != null)
                    socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

```

主函数

```java
import java.io.*;
import java.net.*;

public class MultiThreadEchoServer {
    public static void main(String[] args) throws Exception{
        //开始监听
        ServerSocket listenSocket = new ServerSocket(8189);
        Socket socket = null;
        int count = 0;
        System.out.println("Server listening at 8189");

        while(true){
            //取出一个连接记录
            socket = listenSocket.accept();
            count++;
            System.out.println("The total number of clients is "+count+".");
            ServerThread serverThread = new ServerThread(socket);
            serverThread.start();
        }
    }
}
```

## Socket 客户端

由于老师给的实例没有发送，没法测试服务器是否正常工作，写了一个很简单的客户端

```java
import java.io.*;
import java.net.*;

public class SendThread extends Thread {
    Socket socket = null;

    public SendThread(Socket socket) {
        this.socket = socket;
    }

    public void run() {
        OutputStream os = null;
        PrintWriter pw = null;
        try {
            os = socket.getOutputStream();
            pw = new PrintWriter(os);
            pw.println("hello");
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (pw != null)
                    pw.close();
                if (os != null)
                    os.close();
                if (socket != null)
                    socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) throws Exception {
        int threadnum = 10;
        for (int i = 0; i < threadnum; ++i) {
            Socket socket = new Socket("localhost", 8190);
            System.out.println("Server send to 8190");
            SendThread serverThread = new SendThread(socket);
            serverThread.start();
        }
    }
}
```

## 线程池

ThreadPoolExecutor 线程池

 public ThreadPoolExecutor(int corePoolSize, 

​            int maximumPoolSize,

​            long keepAliveTime,

​            TimeUnit unit,

​            BlockingQueue<Runnable> workQueue,

​            ThreadFactory threadFactory,

​            RejectedExecutionHandler handler)

参数说明

（1）corePoolSize 核心线程数量

即使没有任务执行，核心线程也会一直存活

线程数小于核心线程时，即使有空闲线程，线程池也会创建新线程执行任务

设置allowCoreThreadTimeout=true时，核心线程会超时关闭

（2）maximumPoolSize 最大线程数

当所有核心线程都在执行任务，且任务队列已满时，线程池会创建新线程执行任务。

当线程数=maxPoolSize,且任务队列已满，此时添加任务时会触发RejectedExecutionHandler进行处理。

（3）keepAliveTime TimeUnit 线程空闲时间

如果线程数>corePoolSize，且有线程空闲时间达到keepAliveTime时，线程会销毁，直到线程数量=corePoolSize

如果设置allowCoreThreadTimeout=true时，核心线程执行完任务也会销毁直到数量=0

```java
import java.util.concurrent.*;

public class ThreadPoolTest {
    //5个核心线程，10最大线程
    public static void main(String[] args){
        ThreadPoolExecutor executor = new ThreadPoolExecutor(5, 10, 200, TimeUnit.MILLISECONDS, new ArrayBlockingQueue<>(5));
        //TimeUnit.MILLISECONDES颗粒度
        for(int i = 0; i < 15; i++){
            MyTask myTask = new MyTask(i);
            executor.execute(myTask);
            System.out.println("The number of threads in the ThreadPool:"+executor.getPoolSize());
            System.out.println("The number of tasks in the Queue:"+executor.getQueue().size());
            System.out.println("The number of tasks completed:"+executor.getCompletedTaskCount());
        }
        executor.shutdown();
        
    }
}

class MyTask implements Runnable {
    private int taskNum;
    public MyTask(int num){
        this.taskNum = num;
    }
	// 简单的求和
    @Override
    public void run(){
        int sum = 0;

        System.out.println("Task" +taskNum+"is running!");
        
        try {
            for(int i = 0; i < 15; i++)
                sum += i;
            Thread.currentThread().sleep(4000);
        } catch(InterruptedException e){
            e.printStackTrace();
        }
        System.out.println("Task "+taskNum+" has been done!");
    }
}
```



# maven

```mvn
#在目录下构建项目
$ mvn clean package
#在target/classes下运行
$ java com.xdu.App
```

maven报错 不再支持原选项5

```
在prom中添加
<properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
        <java.version>11</java.version>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
</properties>

```

