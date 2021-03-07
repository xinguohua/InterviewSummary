# Java I/0
## NIO和BIO的区别 

   * IO的方式通常分为几种，同步阻塞的BIO、同步非阻塞的NIO、异步非阻塞的AIO。

   * BIO全称是Blocking IO，是JDK1.4之前的传统IO模型，本身是同步阻塞模式。 线程发起IO请求后，一直阻塞IO，直到缓冲区数据就绪后，再进入下一步操作。

   * NIO也叫Non-Blocking IO 是同步非阻塞的IO模型。线程发起io请求后，立即返回（非阻塞io）。**同步**指的是必须等待IO缓冲区内的数据就绪，而**非阻塞**指的是，用户线程不原地等待IO缓冲区，可以先做一些其他操作，但是要**定时轮询检查IO缓冲区数据**是否就绪。IO多路复用模型中，将检查IO数据是否就绪的任务，**交给系统级别的select或epoll模型**，由系统进行监控，减轻用户线程负担。

   * BIO只能接收一个连接，无法处理大量连接。NIO单线程处理多个连接。

     [参考博客](https://juejin.cn/post/6844903985158045703)

## NIO的BIO的适用场景 

   * BIO方式适用于连接数目比较少且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，程序只管简单易理解。

   * NIO方式适用于连接数目多且比较短的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。

     [参考博客](https://my.oschina.net/u/4006148/blog/3163873)

## NIO的性能瓶颈在哪?
   网络IO往往是性能瓶颈,CPU和磁盘或许仍然会比网络速度快几个数量级。在这种情况下，你绝对不希望让异常快速的CPU等待相对缓慢的网络。
   [参考博客](http://arganzheng.life/java-nio.html)

## epoll

**epoll是linux环境下的一种IO多路复用机制。**（共有3种实现方式：select,poll,epoll）

IO多路复用是一种同步IO模型，实现一个线程可以监视多个文件句柄；一旦某个文件句柄就绪，就能够通知应用程序进行相应的读写操作；没有文件句柄就绪时会阻塞应用程序，交出cpu。多路是指网络连接，复用指的是同一个线程。

**epoll**可以监视的文件描述符数量突破了1024的限制（十万）**，同时**不需要通过轮询遍历的方式去检查文件描述符上是否有事件发生，因为epoll_wait返回的就是有事件发生的文件描述符。本质上是事件驱动。

具体是通过**红黑树和就绪链表**实现的，红黑树存储所有的文件描述符，就绪链表存储有事件发生的文件描述符；

- **epoll_ctl**可以对文件描述符结点进行增、删、改、查，并且**告知内核注册回调函数（事件）**。
- 一旦**文件描述符上有事件发生时，那么内核将该文件描述符节点插入到就绪链表里面**
- 这时候epoll_wait将会接收到消息，并且**将数据拷贝到用户空间**。

[参考](https://blog.csdn.net/daaikuaichuan/article/details/88735256)



## **Java 中有几种类型的流？**


按照流的方向：输入流（inputStream）和输出流（outputStream）

 

按照实现功能分：节点流（可以从或向一个特定的地方（节点）读写数据。如 FileReader）和处理流（是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。如 BufferedReader。处理流的构造方法总是要带一个其他的流对象做参数。一个流对象经过其他流的多次包装，称为流的链接。）

 

按照处理数据的单位： 字节流和字符流。字节流继承于 InputStream 和 OutputStream， 字符流继承于InputStreamReader 和 OutputStreamWriter 。
![I/0](http://www.bjpowernode.com/Public/Uploads/index/itArticle/20190509/1557370976@f847084bda30e33e8c628fa873d09aa6.png)



## **有哪些常见的 IO 模型?**   

UNIX 系统下， IO 模型一共有 5 种： 同步阻塞 I/O、同步非阻塞 I/O、I/O 多路复用、信号驱动 I/O 和异步 I/O。Java 中 3 种  

**(1) BIO (Blocking I/O) 同步阻塞 IO 模型**

同步阻塞 IO 模型中，应用程序发起 read 调用后，会一直阻塞，直到在内核把数据拷贝到用户空间。

![BIO](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6a9e704af49b4380bb686f0c96d33b81~tplv-k3u1fbpfcp-watermark.image)

在客户端连接数量不高的情况下，是没问题的。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。


**(2) NIO (Non-blocking/New I/O) 同步非阻塞 IO 模型**

Java 中的 NIO 于 Java 1.4 中引入，对应 java.nio 包，提供了 Channel , Selector，Buffer 等抽象。NIO 中的 N 可以理解为 Non-blocking，不单纯是 New。它支持面向缓冲的，基于通道的 I/O 操作方法。 对于高负载、高并发的（网络）应用，应使用 NIO 。

Java 中的 NIO 可以看作是 I/O 多路复用模型。也有很多人认为，Java 中的 NIO 属于同步非阻塞 IO 模型。
![BIO](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bb174e22dbe04bb79fe3fc126aed0c61~tplv-k3u1fbpfcp-watermark.image)

同步非阻塞 IO 模型中，应用程序会一直发起 read 调用，等待数据从内核空间拷贝到用户空间的这段时间里，线程依然是阻塞的，直到在内核把数据拷贝到用户空间。
相比于同步阻塞 IO 模型，同步非阻塞 IO 模型确实有了很大改进。通过轮询操作，避免了一直阻塞。  
但是，这种 IO 模型同样存在问题：应用程序不断进行 I/O 系统调用轮询数据是否已经准备好的过程是十分消耗 CPU 资源的。

这个时候，I/O 多路复用模型就上场了。
IO 多路复用模型中，线程首先发起 select 调用，询问内核数据是否准备就绪，等内核把数据准备好了，用户线程再发起 read 调用。read 调用的过程（数据从内核空间->用户空间）还是阻塞的。

目前支持 IO 多路复用的系统调用，有 select，epoll 等等。select 系统调用，是目前几乎在所有的操作系统上都有支持

select 调用 ：内核提供的系统调用，它支持一次查询多个系统调用的可用状态。几乎所有的操作系统都支持。
epoll 调用 ：linux 2.6 内核，属于 select 调用的增强版本，优化了 IO 的执行效率。
IO 多路复用模型，通过减少无效的系统调用，减少了对 CPU 资源的消耗。

Java 中的 NIO ，有一个非常重要的选择器 ( Selector ) 的概念，也可以被称为 多路复用器。通过它，只需要一个线程便可以管理多个客户端连接。当客户端数据到了之后，才会为其服务。



**(3) AIO (Asynchronous I/O)**

AIO 也就是 NIO 2。Java 7 中引入了 NIO 的改进版 NIO 2,它是异步 IO 模型。

异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。

![BIO](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3077e72a1af049559e81d18205b56fd7~tplv-k3u1fbpfcp-watermark.image)


目前来说 AIO 的应用还不是很广泛。Netty 之前也尝试使用过 AIO，不过又放弃了。这是因为，Netty 使用了 AIO 之后，在 Linux 系统上的性能并没有多少提升。

最后，来一张图，简单总结一下 Java 中的 BIO、NIO、AIO。
![BIO](https://images.xiaozhuanlan.com/photo/2020/33b193457c928ae02217480f994814b6.png)

## **字节流如何转为字符流？**


字节输入流转字符输入流通过 InputStreamReader 实现，该类的构造函数可以传入 InputStream 对象。

 

字节输出流转字符输出流通过 OutputStreamWriter 实现，该类的构造函数可以传入 OutputStream 对象。

## **如何将一个 java 对象序列化到文件里？**


在 java 中能够被序列化的类必须先实现 Serializable 接口，该接口没有任何抽象方法只是起到一个标记作用。


```java
public class Test {
    public static void main(String[] args) throws Exception {
        //对象输出流
        ObjectOutputStream objectOutputStream =
                new ObjectOutputStream(new FileOutputStream(new File("D://obj")));
        objectOutputStream.writeObject(new User("zhangsan", 100));
        objectOutputStream.close();
        //对象输入流
        ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream(new File("D://obj")));
        User user = (User) objectInputStream.readObject();
        System.out.println(user);
        objectInputStream.close();
    }
}
```

## **字节流和字符流的区别？**


字节流读取的时候，读到一个字节就返回一个字节；字符流使用了字节流读到一个或多个字节（中文对应的字节数是两个，在 UTF-8 码表中是 3 个字节）时。先去查指定的编码表，将查到的字符返回。字节流可以处理所有类型数据，如：图片，MP3，AVI视频文件，而字符流只能处理字符数据。只要是处理纯文本数据，就要优先考虑使用字符流，除此之外都用字节流。字节流主要是操作 byte 类型数据，以 byte 数组为准，主要操作类就是 OutputStream、InputStream字符流处理的单元为 2 个字节的 Unicode 字符，分别操作字符、字符数组或字符串，而字节流处理单元为 1 个字节，操作字节和字节数组。所以字符流是由 Java 虚拟机将字节转化为 2 个字节的 Unicode 字符为单位的字符而成的，所以它对多国语言支持性比较好！如果是音频文件、图片、歌曲，就用字节流好点，如果是关系到中文（文本）的，用字符流好点。在程序中一个字符等于两个字节，java 提供了 Reader、Writer 两个专门操作字符流的类。

 

 

**6、如何实现对象克隆？**


有两种方式：

 

● 实现 Cloneable 接口并重写 Object 类中的 clone()方法；

 

● 实现 Serializable 接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆，代码如下：

``` java
class MyUtil {
    private MyUtil() {
        throw new AssertionError();
    }
    public static <T extends Serializable> T clone(T obj) throws Exception {
        ByteArrayOutputStream bout = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bout);
        oos.writeObject(obj);
        ByteArrayInputStream bin = new ByteArrayInputStream(bout.toByteArray());
        ObjectInputStream ois = new ObjectInputStream(bin);
        return (T) ois.readObject();
        // 说明：调用 ByteArrayInputStream 或 ByteArrayOutputStream 对象的 close 方法没有任何意义
        // 这两个基于内存的流只要垃圾回收器清理对象就能够释放资源，这不同于对外部资源（如文件流）的释放
    }
}
```

测试代码：
 ```java 
import java.io.Serializable;

/**
 * 人类
 */
class Person implements Serializable {
    private static final long serialVersionUID = -91020170202878978L;
    private String name; // 姓名
    private int age; // 年龄
    private Car car; // 座驾

    public Person(String name, int age, Car car) {
        this.name = name;
        this.age = age;
        this.car = car;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Car getCar() {
        return car;
    }

    public void setCar(Car car) {
        this.car = car;
    }

    @Override
    public String toString() {
        return "Person [name=" + name + ", age=" + age + ", car=" + car + "]";
    }
}
 ```
 * 小汽车类
``` 
class Car implements Serializable {
    private static final long serialVersionUID = -57138907627603702L;
    private String brand; // 品牌
    private int maxSpeed; // 最高时速

    public Car(String brand, int maxSpeed) {
        this.brand = brand;
        this.maxSpeed = maxSpeed;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public int getMaxSpeed() {
        return maxSpeed;
    }

    public void setMaxSpeed(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }

    @Override
    public String toString() {
        return "Car [brand=" + brand + ", maxSpeed=" + maxSpeed + "]";
    }
}

class CloneTest {
    public static void main(String[] args) {
        try {
            Person p1 = new Person("dujubin", 33, new Car("Benz", 300));
            Person p2 = MyUtil.clone(p1); // 深度克隆
            p2.getCar().setBrand("BYD");
            // 修改克隆的 Person 对象 p2 关联的汽车对象的品牌属性
            // 原来的 Person 对象 p1 关联的汽车不会受到任何影响
            // 因为在克隆 Person 对象时其关联的汽车对象也被克隆了
            System.out.println(p1);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

注意：基于序列化和反序列化实现的克隆不仅仅是深度克隆，更重要的是通过泛型限定，可以检查出要克隆的对象是否支持序列化，这项检查是编译器完成的，不是在运行时抛出异常，这种是方案明显优于使用 Object 类的 clone 方法克隆对象。让问题在编译的时候暴露出来总是好过把问题留到运行时。

 

## 什么是 java 序列化，如何实现 java 序列化？


序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化。可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间。序列化是为了解决在对对象流进行读写操作时所引发的问题。序 列 化 的 实 现 ： 将 需 要 被 序 列 化 的 类 实 现 Serializable 接 口 ， 该 接 口 没 有 需 要 实 现 的 方 法 ， implements Serializable 只是为了标注该对象是可被序列化的，然后使用一个输出流(如：FileOutputStream)来构造一个 ObjectOutputStream(对象流)对象，接着，使用 ObjectOutputStream 对象的 writeObject(Object obj)方法就可以将参数为 obj 的对象写出(即保存其状态)，要恢复的话则用输入流。