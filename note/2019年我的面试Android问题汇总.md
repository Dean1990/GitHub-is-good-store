### 2019年我的面试 Android 问题汇总

#### Activity生命周期

#### Service生命周期

#### 四大组件是什么，以及生命周期

#### Application 的生命周期

#### App 启动过程

#### Mainifest 文件的结构，包含什么

#### 绑定 Service 的代码

#### 通过绑定方式开启的 Service 是会随着宿主的销毁而销毁的

#### Service 的通信

#### ContentProvider,ContentResolver,ContentObserver 的作用与关系

#### GC的工作原理，什么时候工作，怎么判定可回收等

#### 自定义View , 以及Canvas 、Path 等绘图相关

#### Android NDK 开发

#### 进程间通讯

#### 使用的网络请求框架

#### 使用的图片加载框架

#### 用什么数据库，使用数据库的场景，多种数据库的对比

#### 数据库中有大量数据时，数据库操作慢，数据库层面如何优化，应用层面如何优化

#### 开发中使用的第三方框架

#### 数据库框架 realm 的优缺点

#### 数据库框架 realm 的使用

#### 数据库框架 realm 的存储形式是 sql 吗

#### 使用的设计模式 MVC与MVP与MVVC的区别，有什么优缺点

#### 如何下载一个大文件

#### 对面向对象的理解

#### 分别说说 封包，继承和多态

#### 网络的七层结构

| OSI七层模型 | 功能                                   | 协议                                                        |                                        | TCP/IP五层模型 | TCP/IP四层模型 |
| ----------- | -------------------------------------- | ----------------------------------------------------------- | -------------------------------------- | -------------- | -------------- |
| 应用层      | 文件传输，电子邮件，文件服务，虚拟终端 | TFTP , HTTP , SNMP , FTP , SMTP , POP3 , DNS , RIP , Telnet |                                        | 应用层         | 应用层         |
| 表示层      | 数据格式化，代码转换，数据加密         |                                                             | 解决不同系统之间的通信语法问题等       | 应用层         | 应用层         |
| 会话层      | 解除或建立与别的接点的联系             |                                                             | 建立和管理应用程序之间的通信           | 应用层         | 应用层         |
| 传输层      | 提供端对端的接口                       | TCP , UDP                                                   |                                        | 传输层         | 传输层         |
| 网络层      | 为数据包选择路由                       | IP , ICMP , OSPF , BGP , IGMP , ARP , RARP                  | 路由器                                 | 网络层         | 网络层         |
| 数据链路层  | 传输有地址的帧以及错误检测功能         | SLIP , CSLIP , PPP , MTU , ARP , RARP                       | 网桥，交换机                           | 数据链路层     | 网络接口层     |
| 物理层      | 以二进制数据形式在物理媒体上传输数据   | IOS2110 , IEEE802 , IEEE802.2                               | 网上，网线，集线器，中断器，调制解调器 | 物理层         | 网络接口层     |



#### Socket TCP的粘包与半包问题

粘包：x.y个包

半包：0.y个包

出现粘包和半包现象,是因为TCP只有流的概念,没有包的概念。

解决办法

- 使用UDP协议，UDP是按包发送的
- 可以每次发送同样大小的包，过大的包不予发送，过小的包，后面部分用固定的字符'\0'进行填充
- 将流按字符处理，用特定的字符，比如 "\\" , "|" 等 对包进行区分，如两个 "\\" 之前的内容为一个包，当然还要对 包内容中出现的特定字符做转义处理，以做区分。
- 发送方在准备发送数据时，先将数据的长度发送给接收方，接收方通过得到的长度再接收TCP的数据流

#### TCP 与 UDP 的区别

#### 混合开发

#### 视频 m3u8 格式相关

#### 视频 拖拽时，拖拽进度与播放进度的细小偏移 

#### 对应用做过什么性能优化，内存，电池，网络，加载等

#### Android 内存优化相关操作以及检测工具

#### 说说ANR，OOM和内存泄露

- **OOM** 当java进程申请的java空间超过阈值时，就会抛出OOM异常（这个阈值由Android系统dalvik vm heapsize硬性限制，视机型而定），可以通过 `adb shell getprop|grep dalvik.vm.heapsize` 查看
- **绕过dalvik vm heapsize 的限制**
- - **创建子进程** 使用 android:process 标签（会引出一个更棘手的问题，进程间通信）
  - **使用 jni 在 native heap 上申请空间（推荐）** nativeheap 不受 dalvik vm heapsize 的限制，当 native 层申请过多的内存导致 RAM 耗尽时，memory killer 可能会杀掉进程（原则上是先杀掉优先级比较低的进程），造成闪退
  - **使用显存** 比如 OpenGL textures , GraphicBufferAllocator 等 API申请的内存就是显存

#### 内存抖动是什么

#### 内存溢出（OOM）溢出的是哪块内存

#### 说说 java 堆内存的结构以及工作原理

#### java 堆 与 栈 的区别

#### CPU 爆满的原因

#### 对弱网或无网的优化

#### 对接微信与支付宝支付的流程

#### 多线程技术，锁，sleep，wait，notify，join

#### sleep 与 wait 的区别

|         | 归属   | 是否堵塞线程 | 唤醒方法              |
| ------- | ------ | ------------ | --------------------- |
| sleep() | Thread | 是           | 传参设置时间          |
| wait()  | Object | 否           | notify()或notifyAll() |

#### EventBus 的 post 与 poststicky 的实现原理

#### 使用哪家的第三方登录，分享等

#### 地图接入经验

#### 应用的保活

#### 广播（静态，动态，有序，无序）

#### 分辨率适配

#### 机型适配（不同的rom,不同的版本）

#### View 点击事件的分发机制，与OnClickListener 的优先级

#### 怎样解决滑动冲突

#### merge 与 ViewStub 的使用以及二者是否可以一起使用

#### Activity 与 Fragment 数据交互

#### 除了子线程与 Handler 这种方式更新 UI ，还有什么其他方式 

#### RxJava 的使用，如何切换线程

#### 是否做过直播

#### HashMap的内部实现

#### 看过什么源码

#### 理解间复杂度和空间复杂度

#### 线程同步中的操作使用 对象锁还是关键字

#### 都了解什么设计模式，Android都用了什么设计模式

#### 直接上机操作，60分钟内完成一个图片搜索展示APP

#### List,Map,Set的区别

#### 两个List中分别有一些数字，找到两个List中相同的数字

#### 怎么设计一个类，有一个必传参数，N个非必传参数

#### Android 热修补

#### Android DataBinding

#### Android LifeCycle

#### Android LifeData

#### 列举 Android 6.0 ,7.0 , 8.0 , 9.0 新增的权限，以及更新变更

#### AOP

#### 注解技术

#### WebView的封装

#### 当一个页面上面是WebView ，下面是ListView等可滑动的视图时，怎样处理

#### bit 与 Byte 的转换

#### 内置数据类型（基本数据类型）与引用数据类型以及对函数形参的理解

```java
public class Abcd {

    private String str = new String("good");
    private char[] chs = {'a','b','c'};

    public static void main(String[] args) {
        Abcd a = new Abcd();
        a.change(a.str,a.chs);
        System.out.print(a.str);
        System.out.print(" and ");
        System.out.println(a.chs);
    }

    void change(String str,char[] chs){
        str = "bad";
        chs[0] = 'g';
    }
}
```

输出结果：`good and gbc`

#### 看过什么源码，Android系统的，第三方框架的

#### Kotlin 中 `var a : String =? null` 中的 `？` 的原理是什么

#### App 弱网络，无网络的处理 及网络优化相关

#### App 的启动优化，黑屏，白屏，慢 等等

#### 多进程下的Application

#### Context 是什么，Application Context 与 Activity Context 的区别

#### Linux 命令，查找所有以 `.svn` 结尾的文件，并删除

#### 十进制转换二进制

#### `int a = 4` 在内存中是怎么存储的

#### 将一个字符串中的字母和数字进行排序

 #### Dagger

#### Okhttp、Retrofit

#### 已上线的项目，当数据库要更改字段类型，怎么办

#### 进程与线程

#### Window、Activity 、ViewRoot 和 DecorView 的区别与联系

#### 点击事件先经过Window还是Activity

#### 下拉刷新与上拉加载的实现，以及自定义刷新头与加载尾

#### 模块化开发

#### Activity 的生命周期是由谁管理的

#### Java volatile 关键字

#### Android 启动模式，以及结合Activity的生命周期

#### 两个Activity，A启动B的过程的生命周期

A onPause()	->	B onCreate()	->	B onStart()	->	B onResume()	->	A onStop()

#### Fragment 生命周期

创建

- onAttach()	->	onCreate()	->	onCreateView()
- onViewCreated() [非主要] 
- onActivityCreated()	->	 onStart()	->	onResume()

失去焦点 部分可见

- onPause()
- onSaveInstanceState() [非主要]

由失去焦点 部分可见 到活动（获得焦点）

- onResume()

不可见

- onPause()	->	onStop()
- onSaveInstanceState() [非主要]

由不可见到活动（获得焦点）

- onStart()	-> onResume()

被回收

- onPause()	->	onStop() 
- onSaveInstanceState() [非主要] 
- onDestroyView()    ->    onDestroy()    ->    onDetach()

回收后重新创建

- onAttach()	->	onCreate()	->	onCreateView()
- onViewCreated() [非主要] 
- onActivityCreated()
- onViewStateRestored() [非主要]
- onStart()	->	onResume()

横竖屏切换 同被回收重新创建

退出应用

- onPause	->	onStop()	->	onDestroyView()	->	onDestroy()	->	onDetach()

退出不会调用 onSaveInstanceState() 方法，因为是人为退出，不需要保存数据

注：setUserVisibleHint() [非生命周期方法] 在 ViewPage + Fragment 相结合使用时会被调用在 onCreateView() 方法之前，其他情况需要手动调用。

#### Fragment 是怎样添加的，FragmentManager 是哪个包里的



#### 都用过 Map 的哪些实现类

#### HashMap 与 HashTable 的区别

#### 链表是什么

#### 代码实现优先级队列

#### 如果一个链表中可能有循环，如果在不通过使用其他数据结构的情况下判断是否有循环

#### 启动页白屏的处理

#### 在 Application 中有一个耗时初始化A，MainActivity 中要使用 A，如何做到不影响 A 在 MainActivity 中的使用

首先耗时操作，需要对A的初始化开启线程，并发操作下，当进入MainActivity 中使用A时，不能确定A当前已经初始化完成，需要使用 `Thread.join()` 或者 `CountDownLatch`  

#### 都用过什么同步方式

#### manifest 中的 tastAffinity 属性是做什么的

1.affinity是Activity内的一个属性（在Manifest中对应属性为taskAffinity）。默认情况下，拥有相同affinity的Activity属于同一个Task中。

2.Task也有affinity属性，它的affinity属性由根Activity（创建Task时第一个被压入栈的Activity）决定。

3.在默认情况下（我们什么都不设置），所有的Activity的affinity都从Application继承。也就是说Application同样有taskAffinity属性。

```
<application
	android:taskAffinity="gf.zy"
```
4.Application默认的affinity属性为Manifest的包名。

#### 冷启动和热启动

#### 与H5的交互

#### String , StringBuilder , StringBuffer 的区别

#### 手写单例的代码，单例的几种形式

#### 断点续传的实现

#### 长连接

#### HandlerThread

#### ThreadLocal

#### IntentService

#### Handler机制

#### Looper 是个死循环，主线程的 Looper 为什么不会造成堵塞

#### MessageQueue 存放的是 Ｍessage 是如何调用 handleMessage()方法的

通过 Looper 的 loop() 方法，一直从 MessageQueue 中取出 Message ， Message的 target 中存在着发送这个 Message 的 Handler 

```java
msg.target.dispatchMessage(msg);//将消息发送到主线程，调起handleMessage()方法
```

```java
public void dispatchMessage(Message msg) {
        if (msg.callback != null) {
            handleCallback(msg);
        } else {
            if (mCallback != null) {
                if (mCallback.handleMessage(msg)) {
                    return;
                }
            }
            handleMessage(msg);
        }
    }
```

 这里的 target 是在 Handler 的 sendMessage() 的时候添加的

```java
public void sendMessage(Message msg){
        msg.target = this;
        mQueue.enqueueMessage(msg);
}
```

#### Looper 的 prepare() 做了什么

Looper对象的初始化，并将其放到 ThreadLocal中保存

```java
public static void prepare(){
        if (sThreadLocal.get()!=null){
            throw new RuntimeException("Only one Looper may be created per thread");
        }
        sThreadLocal.set(new Looper());
}
```

#### MessageQueue 是在哪个类里，是什么时候创建的

消息队列是在 Looper 类中创建的

```java
private Looper(){
        mQueue = new MessageQueue();
}
```

#### MessageQueue 消息队列是有序的吗，延时消息是怎么处理的

消息队列是单链表形式，有序。

进入消息队列的消息会计算出相对于系统开机时间多少秒后执行的时间，参数 `when` ，并与已经在消息队列中的消息做比较，进行排序插入相应的位置。

在消息轮训的时候，`next()` 方法会先判断位于队首的消息 `when` 是否需要延迟，如果延迟则会调用  `nativePollOnce()` 方法，类似于 `object.wait()` ，线程堵塞。当有新的消息进入消息队列时，会唤起线程。

#### ReentrantLock

#### 线程池，常用线程池

#### Java 四种引用类型

|        | 创建                                                         | GC                                                           | 应用                         |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------------------- |
| 强引用 | Java中默认声明的对象  new T()                                | 永远不会主动回收，内存不足，JVM报OOM，需要设置对象为null，GC才会回收 |                              |
| 软引用 | new SoftReference<T>(T)                                      | 当内存不足时，会回收软引用对象，但当回收后，内存还是不足时，报OOM | 缓存技术等                   |
| 弱引用 | new WeakReference<T>(T)                                      | 无论内存是否足够，每次都会被GC回收                           | 防止内存泄露等               |
| 虚引用 | new PhantomReference<T>(T,ReferenceQueue)  需要与 引用队列（ReferenceQueue）一起使用 | get 方法永远返回null，随时可能会被回收                       | GC时，用于观察对象是否被回收 |

虚引用的作用在于跟踪垃圾回收过程，在对象被收集器回收时收到一个系统通知。 当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在垃圾回收后，将这个虚引用加入引用队列，在其关联的虚引用出队前，不会彻底销毁该对象。 所以可以通过检查引用队列中是否有相应的虚引用来判断对象是否已经被回收了。

如果一个对象没有强引用和软引用，对于垃圾回收器而言便是可以被清除的，在清除之前，会调用其finalize方法，如果一个对象已经被调用过finalize方法但是还没有被释放，它就变成了一个虚可达对象。

与软引用和弱引用不同，显式使用虚引用可以阻止对象被清除，只有在程序中显式或者隐式移除这个虚引用时，这个已经执行过finalize方法的对象才会被清除。想要显式的移除虚引用的话，只需要将其从引用队列中取出然后扔掉（置为null）即可。

#### Thread怎么关闭

#### Gradle 的使用，批量打包，打包压缩，等，Gradle 代码的编写





- 做自我介绍，说说都做过什么项目

- 在做过的项目中，有什么技术难点

- 工作几年，Android 经验几年，期望薪资，

- 是否有可以展示的项目

- 在这个项目中负责哪部分，几个人，用时

- 说说研发的组织架构，人员构成

- 离职原因，上家公司工资，税前，税后

- 5G 对 Android 是否会有影响

- 你想了解我们公司什么

- 家住哪里，路程需要多长时间

- 是否结婚，是否打算要小孩

- 是否离职，社保交到几月，多长时间可以到岗

- 当我们使用第三方开源框架时，如果发现未知问题，当不确定是否是框架的问题时，怎么办

  主要考察的点是“开源”，最快的方式是去搜索开源项目的问题列表（Issues），次之，查看源码；

  当确定是框架问题时，最好的解决办法是，检查版本，作者可能会在最新的版本已经解决该问题，其次，Fork 修改源码，最后考虑换框架（代价最大）

- 英文面试题

