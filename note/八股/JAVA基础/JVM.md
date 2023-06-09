#### 1.JVM的位置

![image-20221123003548602](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221123003548602.png)





#### 2.JVM的体系结构

![image-20221123004704278](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221123004704278.png)

 JVM[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)模型包括：程序计数器、本地方法栈、虚拟机堆(线程)、线程栈、方法区(元空间)，程序计数器、线程栈、本地方法栈是每个线程所独有的。



#### 3.类加载器

作用：加载Class文件~ 

![image-20221123005945073](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221123005945073.png)

1.虚拟机自带的加载器

2.启动类(根)加载器

3.扩展类加载器

4.应用程序加载器

![image-20221124002126396](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221124002126396.png)





![image-20221126091309556](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221126091309556.png)



#### 4.双亲委派机制

 向上请求，向下加载

1．类加载器收到类加载的请求!

2．将这个请求向上委托给父类加载器去完成，一直向上委托，知道启动类加载器

3、启动加载器检查是否能够加载当前这个类，能加载就结束，使用当前的加载器，否则，抛出异常，通知子加载器进行加软

4.重复步骤3

Class Not Found ~

null：java调用不到~ c、c++



#### 5.沙箱安全机制

​		Java安全模型的核心就是Java沙箱(sandbox)，什么是沙箱?沙箱是一个限制程序运行的环境。沙箱机制就是将Java代码限定在虚拟机(JVM)特定的运行范围中，并且严格限制代码对本地系统资源访问，通过这样的措施来保证对代码的有效隔离，防止对本地系统造成破坏。沙箱**主要限制系统资源访问**，那系统资源包括什么? CPU、内存、文件系统、网络。不同级别的沙箱对这些资源访问的限制也可以不一样。

​		所有的Java程序运行都可以指定沙箱，可以定制安全策略。

​		在Java中将执行程序分成本地代码和远程代码两种，本地代码默认视为可信任的，而远程代码则被看作是不受信的。对于授信的本地代码，可以访问一切本地资源。而对于非授信的远程代码在早期的lava实现中，安全依赖于沙箱(Sandbox)机制。如下图所示JDK1.0安全模型

![image-20221123233409105](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221123233409105.png).

​		但如此严格的安全机制也给程序的功能扩展带来障碍，比如当用户希望远程代码访问本地系统的文件时候，就无法实现。因此在后续的Java1.1版本中，针对安全机制做了改进，增加了安全策略，允许用户指定代码对本地资源的访问权限。如下图所示JDK1.1安全模型。

![image-20221123233538507](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221123233538507.png)

​		在Java1.2版本中，再次改进了安全机制，增加了代码签名。不论本地代码或是远程代码，都会按照用户的安全策略设定，由类加载器加载到虚拟机中权限不同的运行空间，来实现差异化的代码执行权限控制。如下图所示JDK1.2安全模型

![image-20221123233631483](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221123233631483.png)

​		当前最新的安全机制实现，则引入了域(Domain)的概念。虚拟机会把所有代码加载到不同的系统域和应用域，系统域部分专门负责与关键资源进行交互，而各个应用域部分则通过系统域的部分代理来对各种需要的资源进行访问。虚拟机中不同的受保护域(Protected Domain)，对应不一样的权限(Permission)。存在于不同域中的类文件就具有了当前域的全部权限，如下图所示最新的安全模型(jdk 1.6)

![image-20221123233757448](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221123233757448.png)

##### 组成沙箱的基本组件：

**&**字节码校验器(bytecode verifier)︰确保Java类文件遵循Java语言规范。这样可以帮助ava程序实现内存保护。但并不是所有的类文件都会经过字节码校验，比如核心类。

**&**类装载器(class loader) :其中类装载器在3个方面对Java沙箱起作用

​	它防止恶意代码去干涉善意的代码;

​	它守护了被信任的类库边界;

​	它将代码归入保护域，确定了代码可以进行哪些操作。

​		虚拟机为不同的类加载器载入的类提供不同的命名空间，命名空间由一系列唯一的名称组成，每一个被装载的类将有一个名字，这个命名空间是由Java虚拟机为每一个类装载器维护的，它们互相之间甚至不可见。



#### 6.Native

​		凡是带了native 关键字的，说明java的作用范围达不到了，回去调用底层c语言的库！会进入本地方法栈，调用本地方法接口（JNI）

JNI作用：扩展JAVA的使用，融合不同的编程语言为JAVA所用！最初：c，c++。

它在内存区城中专门开眸了一块标记区域: Native Method Stack,登记native方法，在最终执行的时候，加载本地方法库中的方法通过JNI



#### 7.PC寄存器

程序计数器:Program Counter Register

​		每个线程都有一个程序计数器，是线程私有的，就是一个指针，指向方法区中的方法字节码（用来存储指向像一条指令的地址，也即将要执行的指令代码)，在执行引擎读取下一条指令，是一个非常小的内存空间，几乎可以忽略不计

#### 8.方法区

Method Area方法区

​		方法区是被所有线程共享，所有字段和方法字节码，以及一些特殊方法，如构造函数，接口代码也在此定义，简单说，所有定义的方法的信息都保存在该区域，此区域属于共享区间;

​		<font color='red'>静态变量、常量、类信息(构造方法、接口定义)、运行时的常量池存在方法区中，但是实例变量存在堆内存中，和方法区无关</font>

static,final,Class模板,常量池

#### 9.栈

先进后出，后进先出

为什么main()先执行，最后结束

栈：栈内存，主管程序的运行，生命周期和线程同步;
线程结束，栈内存也就是释放，对于栈来说，**不存在垃圾回收问题**

一旦线程结束，栈就over

栈：8大基本类型+对象的引用+实例的方法

栈运行原理：栈帧

栈满了，StackOverflowError

栈+堆+方法区：交互关系

![image-20221124002450145](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221124002450145.png)





#### 10.三种JVM

![image-20221124003305280](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221124003305280.png)

#### 11.堆

Heap，一个JVM只有一个堆内存，堆内存的大小是可以调节的

​		类加载器读取了类文件后，一般会把什么东西放到堆中？类，方法，常量，变量~，保存我们所有引用类型的真实对象

堆内存中还要细分为三个区域：

​		**·**	新生区(伊甸园区) Young/New

​		**·**	养老区	Old

​		**·**	永久区

![image-20221124004441978](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221124004441978.png)

GC垃圾回收，主要是在伊甸园区和养老区~

假设内存满了、OOM，堆内存不够！

在JDK8以后，永久存储区改了个名字（元空间）

​			

#### 12.新生区，老年区

新生区：

​	**·** 类单程和成长的地方，甚至死亡；

​	**·** 伊甸园，所有的对象都是在伊甸园区new出来的！

​	**·** 幸存者区(0,1)

真理：经过研究，99%的对象



#### 13.永久区

​		这个区域常驻内存的。用来存放jdk自身携带的Class对象。Interface元数据，存储的是java运行时的一些环境或者类信息~，这个区域不存在垃圾回收！关闭JVM虚拟机就会释放这个区域的内存~

​		一个启动类，加载了大量的第三方jar包。Tomcat部署了太多的应用，大量动态生成的反射类。不断的被加载。直到内存满，就会出现OOM;

​			**·** jdk1.6之前：永久代，常量池是在方法区；

​			**·** jdk1.7		：永久代，但是慢慢的退化了，去永久代，常量池在堆中

​			**·** jdk1.8之后 ：无永久代，常量池在元空间

![image-20221124181608256](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221124181608256.png)

逻辑上存在，物理上不存在

.默认情况下:分配的总内存是电脑内存的1/4，而初始化的内存:1/64

#### 14.堆内存调优





#### 15.GC垃圾回收

JVM在进行GC时，并不是对这三个区域统一回收。大部分时候，回收都是新生代~

**·** 新生区

**·** 幸存区

**·** 老年区

GC两种类：轻GC（普通的GC) ，重GC(全局GC)



#### 16.GC常用算法

GC的算法有哪些? 引用计数法，复制算法，标记清除算法，标记压缩，怎么用的?

##### 	引用计数法

![image-20221125002330451](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221125002330451.png)



##### 	复制算法

![image-20221125004651293](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221125004651293.png)

​	![image-20221125005207711](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221125005207711.png)



好处：没有奴才能的碎片~

坏处：浪费了内存空间~: 多了一半空间永远是空to。假设对象100%存活(极端情况)

复制算法最佳使用场景:对象存活度较低的时候：新生区

##### 标记清除算法

![image-20221125005744166](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221125005744166.png)

优点：不需要额外的空间！

缺点：两次扫描，严重浪费时间，会产生内存碎片。



##### 标记压缩

![image-20221125010056269](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221125010056269.png)

##### 总结

内存效率：复制算法>标记清除算法>标记压缩算法(时间复杂度)

内存整齐度：复制算法=标记压缩算法>标记清除算法

内存利用率：标记压缩算法=标记清除算法>复制算法

思考一个问题：难道没有最优算法吗?
答案:没有，没有最好的算法，只有最合适的算法----->GC:分代收集算法



**年轻代:**
	存活率低-----复制算法!



**老年代:**
·区域大: 存活率
·标记清除(内存碎片不是太多)+标记压缩混合实现





#### 17.JMM

1.什么是JMM

Java Memory Mode--------java内存模型

2.它干嘛的?︰官方，其他人的博客，对应的视频!

作用：缓存一致性协议，用于定义数据读写的规则（遵守，找到这个规则)。

JMM定义了线程工作内存和主内存之间的抽象关系∶线程之间的共享变量存储在主内存(Main Memory)中，每个线程都有一个私有的本地内存(Local Memory)

![image-20221125012243968](C:\Users\WZ\AppData\Roaming\Typora\typora-user-images\image-20221125012243968.png)

解决共享对象可见性这个问题: volilate



#### jvm工作原理？了解过jvm调优吗？有哪些分区？

JVM（Java虚拟机）是Java程序运行的环境，负责将Java字节码文件解释或者编译成机器码并执行。JVM的工作原理包括以下几个方面：

1. 类加载：JVM通过类加载器（ClassLoader）将Java字节码文件加载到内存中，并转换成JVM内部的数据结构，例如类对象（Class Object）和方法区（Method Area）。
2. 内存管理：JVM负责管理Java程序的内存，包括堆内存（Heap Memory）和栈内存（Stack Memory）。堆内存用于存储Java对象实例，栈内存用于存储方法调用的局部变量和操作数栈。
3. 垃圾回收：JVM通过垃圾回收器（Garbage Collector）自动回收不再使用的对象，释放内存资源，避免内存泄漏和溢出。
4. 即时编译：JVM在运行时对热点代码进行即时编译（Just-In-Time Compilation），将频繁执行的字节码转换成机器码，提高程序的执行效率。

JVM调优是指通过优化JVM的配置和参数设置，以提高Java程序的性能和稳定性。常见的JVM调优包括以下几个方面：

1. 堆内存设置：合理配置堆内存的大小，避免内存不足或者过多的情况发生，例如通过-Xmx和-Xms参数设置堆的最大和初始大小。
2. 垃圾回收设置：选择合适的垃圾回收器，配置合理的垃圾回收参数，例如调整新生代和老年代的比例、设置垃圾回收的阈值等。
3. 类加载设置：优化类加载器的加载路径，减少类加载的时间和开销，例如通过设置-classpath参数或者使用自定义的类加载器。
4. 线程管理：合理配置线程池的大小，避免线程过多或者过少导致的性能问题，例如通过设置-Xss参数设置线程栈的大小。

JVM内存分区主要包括堆内存（Heap Memory）和非堆内存（Non-Heap Memory）两部分。堆内存用于存储Java对象实例，可以通过-Xmx和-Xms参数设置其最大和初始大小。非堆内存用于存储JVM和Java程序的元数据和其他信息，包括方法区（Method Area）、虚拟机栈（VM Stack）、本地方法栈（Native Method Stack）等。这些分区在内存管理和垃圾回收时具有不同的特性和行为，对JVM的性能和稳定性有着重要影响。







#### 





单点登陆~SSO













对象实例化的过程



