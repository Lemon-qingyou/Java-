**参考[ Java程序运行机制及其运行过程_小猿备忘录-CSDN博客_java程序运行机制](https://blog.csdn.net/Myuhua/article/details/81301205?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164437288716780265451959%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164437288716780265451959&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-3-81301205.first_rank_v2_pc_rank_v29&utm_term=java%E7%A8%8B%E5%BA%8F%E8%BF%90%E8%A1%8C%E8%BF%87%E7%A8%8B&spm=1018.2226.3001.4187)**

​		**javac程序是一个Java编译器。**它将文件HelloWorld.java编译成HelloWorld.class文件，并发送到java虚拟机（JVM）。虚拟机执行编译器放在class文件中的字节码。

## JVM加载class文件的原理机制

![img](C:\Users\清悠樱落\Documents\Typora文件\Java学习知识\类的加载与ClassLoader的理解\JVM加载class文件的原理机制)

**JVM 中类的装载是由类加载器(ClassLoader)和它的子类来实现的**，Java 中的类加载器是 一个重要的 Java 运行时系统组件，它负责在运行时查找和装入类文件中的类。
由于 Java 的跨平台性，经过编译的 Java 源程序并不是一个可执行程序，而是一个或多个类文件。 当 Java 程序需要使用某个类时，**JVM 会确保这个类已经被加载、连接(验证、准备和解析)和 初始化**。

**类的加载是指把类的.class 文件中的数据读入到内存中**，通常是创建一个字节数组读 入.class 文件，然后产生与所加载类对应的 Class 对象。加载完成后，Class 对象还不完整，所以此时的类还不可用。**当类被加载后就进入连接阶段，这一阶段包括验证、准备(为静态变量分 配内存并设置默认的初始值)和解析(将符号引用替换为直接引用)三个步骤。最后 JVM 对类 进行初始化，包括:1)如果类存在直接的父类并且这个类还没有被初始化，那么就先初始化父类; 2)如果类中存在初始化语句，就依次执行这些初始化语句**。

类的加载是由类加载器完成的，**类加载器包括:根加载器(BootStrap)、扩展加载器(Extension)、 系统加载器(System)和用户自定义类加载器(java.lang.ClassLoader 的子类)**。从 Java 2(JDK 1.2)开始，**类加载过程采取了双亲委派机制(PDM)**。PDM 更好的保证了 Java 平台的安全性， 在该机制中，JVM 自带的 Bootstrap 是根加载器，其他的加载器都有且仅有一个父类加载器。类的加载首先请求父类加载器加载，父类加载器无能为力时才由其子类加载器自行加载。JVM 不会向 Java 程序提供对 Bootstrap 的引用。

下面是关于几个类加载器的说明:

**Bootstrap（根加载器）**:一般用本地代码实现，负责加载 JVM 基础核心类库(rt.jar);
**Extension（扩展加载器）**:从 java.ext.dirs 系统属性所指定的目录中加载类库，它的父加载器是 Bootstrap;

**System:（系统加载器，又叫应用类加载器）**，其父类是 Extension。它是应用最广泛的类加载器。它从环境变量 classpath 或者系统属				性 java.class.path 所指定的目录中记载类，是用户自定义加载 器的默认父加载器。

## java的  “一次编译，到处运行”  的原理

![](C:\Users\清悠樱落\Documents\Typora文件\Java学习知识\类的加载与ClassLoader的理解\一次编译，到处运行.png)虚拟机可以理解成一个以**字节码为机器指令的CPU**。

对于不同的运行平台，有不同的虚拟机。

java虚拟机机制**屏蔽了底层运行平台的差别**，实现了“一次编译，随处运行”。