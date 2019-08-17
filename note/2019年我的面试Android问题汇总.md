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

#### Handler机制

#### Looper 是个死循环，主线程的 Looper 为什么不会造成堵塞

#### 消息队列是有序的吗，延时消息是怎么处理的

#### 使用的网络请求框架

#### 使用的图片加载框架

#### 用什么数据库，使用数据库的场景

#### 数据库中有大量数据时，数据库操作慢，数据库层面如何优化，应用层面如何优化

#### 开发中使用的第三方框架

#### 数据库框架 realm 的优缺点

#### 数据库框架 realm 的使用

#### 数据库框架 realm 的存储形式是 sql 吗

#### 使用的设计模式 MVC与MVP与MVVC的区别

#### 如何下载一个大文件

#### 对面向对象的理解

#### 分别说说 封包，继承和多态

#### 网络的七层结构

#### Socket TCP的粘包问题

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

#### 对弱网或无网的优化

#### 对接微信与支付宝支付的流程

#### 多线程技术，锁，sleep，wait，notify

#### EventBus 的 post 与 poststicky 的实现原理

#### 使用哪家的第三方登录，分享等

#### 地图接入经验

#### 应用的保活

#### 广播（静态，动态，有序，无序）

#### 分辨率适配

#### 机型适配（不同的rom,不同的版本）

#### View 点击事件的分发机制

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

#### 都了解什么设计模式

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

#### Android都用了什么设计模式

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

#### Fragment 生命周期

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

1.affinity是Activity内的一个属性（在ManiFest中对应属性为taskAffinity）。默认情况下，拥有相同affinity的Activity属于同一个Task中。

2.Task也有affinity属性，它的affinity属性由根Activity（创建Task时第一个被压入栈的Activity）决定。

3.在默认情况下（我们什么都不设置），所有的Activity的affinity都从Application继承。也就是说Application同样有taskAffinity属性。

```
<application
	android:taskAffinity="gf.zy"
```
4.Application默认的affinity属性为Manifest的包名。

#### 冷启动和热启动



- 做自我介绍，说说都做过什么项目

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

