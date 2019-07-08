### 性能优化

#### 内存泄漏分析

发生OOM的条件分析，避免内存泄漏（如何使用更高效的 ArrayMap 容器、如何避免不经意的“自动装箱”、Lint , StictMode 等工具的使用技巧）、内存管理机制（共享内存、分配与回收内存、限制应用的内存、应用切换操作）、OOM（查看内存使用情况）、onLowMemory 与 onTrimMemory 的回调

#### 性能优化工具的使用

MAT、LeakCanary、Memory Monitor、Allocation Tracking、Heap Tool、hierarchyviewer 布局检测工具

#### 第三方分析工具

MemoryAnalyzer、GT Home、iTest

#### Android的渲染机制分析

#### 电量优化

分析电量的流失、分析电量消耗数据、分析充电状态和电池管理、蜂窝信号对电量消耗、JobScheduler

#### 网络优化

Batching批处理技术、Prefetching预取技术、GCMNetworkManager高级实践

#### Android Wear上如何做优化

Wearable API、JobScheduler

#### View的性能

自定义View的性能优化、提升View的渲染性能、处理重复layout操作的性能问题

#### Bitmap内存优化

缩放性能优化、缓存性能优化、重用性能优化、PNG压缩性能优化

#### 安装包性能优化

#### 数据传输的效率优化

#### 隐形内存杀手Service的调优

#### 设计线程池优化性能

#### 多线程并发的性能问题

AsyncTask源码级分析及注意、HandlerThread的处理、IntentService使用场景分析和实践、ThreadPool使用场景和注意

#### 程序调优提高应用启动速度

#### Splash页面优化设计的窍门

缓存加载设计、如何提升主界面响应速度

----

### NDK开发

#### C编程

函数、指针（N级指针概念、指针数组、数组指针）、内存布局、结构体和共用体、文件操作、宏、动态库的封装和设计

#### C++编程

C++对C的扩展（C++关键字、命名空间、引用、C/C++混合编程、引用、函数扩展）、C++基础编程（对象管理、类的构造和析构、友元函数与友元类、操作符重载、C++编译器对象管理模型分析、类的继承、多态、抽象类、函数模板、类模板、模板的继承、C++类型转换、C++ IO、异常处理）、C++ STL（序列式容器、堆栈容器、双向链表容器、关联式容器、对组、算法详解）

#### FFmpeg

音视频编码原理、音频解码、视频解码、视频播放器

#### Linux系统编程

Linux基本命令、vim使用、GCC GDB使用、Mikefile、文件I/O操作、Linux文件系统剖析、进程管理（进制控制原语、进程间通信、信号处理、进程间关系和守护进程）、线程控制原语和线程间同步、网络编程（网络编程协议、Socket套接字原语详解）

#### JNI开发

JNI类型、JNI函数操作（数组操作、字符串操作、Java层访问（类、属性、方法））、异常、引用操作（局部引用、全局引用）、优化

#### NDK

运行机制与流程、Android.mk（GNU Make系统变量、模块描述变量、GNU Make功能宏）、Application.mk、日志与调试、支持C++ 、Native原生绘制、OpenSL ES、双进程守护、视频直播（FAAC、X264、RTMP）、视频通话

----

### 移动架构师

#### 创建型模式

Simple Factory、Factory Method、Abstract Factory、Builder、Prototype、Singleton

#### AOP架构设计

Aspect、Joint point、Pointcut、Advice、用户行为统计场景、性能监控场景

#### 行为型模式

Template Method、Observer、State、Strategy、Chain of Responsibility、Command、Visitor、Mediator、Memento、Iterator、Interpreter

#### 网络访问框架设计

#### UML建模

图（类图、时序图）、关系（依赖Dependency、泛化Generalization、关联Association、实现Realization）

#### 设计原则

单一职责SRP、里氏替换LSP、依赖倒置DIP、接口隔离ISP、迪米特LOD、开闭OCP

#### 图片加载框架设计

配置、外观、请求队列、请求、请求转发、加载器、加载策略、缓存策略

#### 结构型模式

Facade、Adapter、Proxy、Decorator、Bridge、Composite、Flyweight

#### IOC架构设计

运行时注入、编译时注入、注入布局、注入视图、注入事件

#### MVP架构

----

### 高级UI开发

#### UI绘制流程分析

源码级分析、View的测量、View的布局、View的绘制过程

#### 绘图及特效制作

Paint画笔高级技能（Paint的方法使用技巧、高级渲染（BitmapShader位图渲染、LinearGradient线性渲染、RadialGradient环形渲染、SweepGradient扫描渐变渲染、ComposeShader组合渲染））、Xfermode、滤镜效果（BlurMaskFilter滤镜、EmbossMaskFilter滤镜）、颜色通道过滤（ColorMatrixColorFilter颜色矩阵过滤、LightingColorFilter曝光颜色过滤、PorterDuffColorFilter图层混合颜色过滤）、Canvas画板高级技能（Canvas基本使用技巧、Canvas区域切割技巧（实例：android实现OS Reveal特效））、Canvas变换使用技巧（translate、scale、rotate、skew斜拉画布）、Canvas图层与状态方法使用技巧（通过save和restore解决图层绘制技术、离屏缓冲技术、PorterDuffColorFilter图层混合颜色过滤）、超强辅助英雄-Path工具类的使用、超强ADC英雄-PathMeasure牛叉辅助类的使用

#### 自定义控件

自绘控件、继承控件、组合控件、Scroller详解及源码浅析、ViewDragHelper详解及源码浅析、自定义View触摸工具类解析（ViewConfiguration基础参数工具类、VelocityTracker手势速率工具类、GestureDetector手势工具类）、大量自定义控件实践（滑动选择价格区间标签控制、热门标签—流式布局、腾讯内部技术-QQ空间之打造个性化可拉伸头部控件、个性化滑动指示器、Material Design—RecyclerView实现时光轴效果、android实现iOS Reveal特效）

#### 事件传递机制（深入源码分析）

#### 事件冲突解决

#### 高级动画及特效

属性动画完全解析、MaterialDesign动画（Touch feedback（触摸反馈）、Reveal effect（揭露效果）、Activity transitions（Activity转换效果）、Curved motion（曲线运动）、View state changes（视图状态改变）、Animate Vector Drawables（矢量动画））、SVG（SVG概述、SVG图片使用实例、SVG动画使用实例）、GIF动画引擎框架、自定义动画框架

#### Material Design原材料设计开发

NavigationView+DrawerLayout主流侧滑实现、TextInputLayout、Snackbar、Toolbar、Material Design样式属性开发、百分比布局、沉浸式设计、TabLayout、Palette调色板、FloatingActionButton悬浮按钮及联动、CollapsingToolbarLayout、自定义Behavior及源码分析



