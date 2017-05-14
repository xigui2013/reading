#第一章
#第二章
##1.运行时数据区域
 1. 程序计数器
 > 当前线程所执行的字节码的行号指示器。每个线程一个，相互独立。Native方法值为空。无OutOfMemoryError异常
 2. Java虚拟机栈
 > 线程私有，存储局部变量表、操作数栈、动态链接、方法出口等信息。StackOverflowError(固定长度虚拟机栈)异常和OutOfMemoryError(动态长度虚拟机栈)
 3. 本地方法栈
 > 服务于native方法。HotSpot直接把本地方法栈和虚拟机栈合二为一。异常通虚拟机栈。
 4. Java堆
 > 存放对象实例。物理上不一定连续，逻辑上连续即可。主流是可扩展的，会有OutOfMemoryError异常。
 5. 方法区
 > 已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。Hotspot中用永久代实现方法区。一般有常量池回收及类卸载。和堆一样，可固定内存或者设置为动态长度，物理上不要求连续。
 6. 运行时常量池
 > 方法区的一部分。编译后的常量存放，运行时生成的常量存放(String的intern方法)。有OutOfMemoryError异常。
 7. 直接内存
 > Java的NIO通过native分配的堆外内存(DirectByteBuffer对象管理)。不受Java堆限制，受限于总机内存，有OutOfMemoryError异常。
 
##2.对象相关
 1. 对象的创建
 > new->检查常量池(类是否已存在)->类加载->分配内存->内存置零->对象设置->后续init相关
 2. 对象的内存布局
 > 1. 对象头(Mark Word,类型指针，数组长度数据)
 > 2. 实例数据
 > 3. 对齐填充
 3. 对象的访问定位
 > 1. 句柄->reference指向句柄池的句柄，句柄包含对象实例数据与类型数据的地址->垃圾回收不改变reference,但多一次中间访问
 > 2. 指针->reference直接指向对象地址，对象中有地址指向对象类型->一次访问对象，速度快，但是垃圾回收相对麻烦
 4. OutOfMemoryError
    ```java
    package gc;
    
    import java.util.ArrayList;
    import java.util.List;
    /**
     * leetcode
     * Created by wjw on 14/05/2017.
     * VM args:-Xms20m-Xmx=20m-XX:HeapDumpOnOutOfMemoryError
     */
    public class HeapOOM {
        static class OOMObject{
        }
        public static void main(String[] args) {
            List<OOMObject> list = new ArrayList<OOMObject>();
            while (true){
                list.add(new OOMObject());
            }
        }
    }
    ```
 输出
 ```java
 /Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/bin/java -Xms20m -Xmx20m -XX:+HeapDumpOnOutOfMemoryError "-javaagent:/Applications/IntelliJ IDEA.app/Contents/lib/idea_rt.jar=58548:/Applications/IntelliJ IDEA.app/Contents/bin" -Dfile.encoding=UTF-8 -classpath /Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/charsets.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/deploy.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/cldrdata.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/dnsns.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/jaccess.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/jfxrt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/localedata.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/nashorn.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/sunec.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/sunjce_provider.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/sunpkcs11.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/ext/zipfs.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/javaws.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/jce.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/jfr.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/jfxswt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/jsse.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/management-agent.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/plugin.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/resources.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/lib/ant-javafx.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/lib/dt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/lib/javafx-mx.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/lib/jconsole.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/lib/packager.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/lib/sa-jdi.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/lib/tools.jar:/Users/wjw/workspaces/leetcode/target/classes:/Users/wjw/.m2/repository/org/jetbrains/annotations-java5/15.0/annotations-java5-15.0.jar gc.HeapOOM
 objc[3083]: Class JavaLaunchHelper is implemented in both /Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/bin/java (0x10299f4c0) and /Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre/lib/libinstrument.dylib (0x102a674e0). One of the two will be used. Which one is undefined.
 java.lang.OutOfMemoryError: Java heap space
 Dumping heap to java_pid3083.hprof ...
 Heap dump file created [27776100 bytes in 0.260 secs]
 Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
 	at java.util.Arrays.copyOf(Arrays.java:3210)
 	at java.util.Arrays.copyOf(Arrays.java:3181)
 	at java.util.ArrayList.grow(ArrayList.java:261)
 	at java.util.ArrayList.ensureExplicitCapacity(ArrayList.java:235)
 	at java.util.ArrayList.ensureCapacityInternal(ArrayList.java:227)
 	at java.util.ArrayList.add(ArrayList.java:458)
 	at gc.HeapOOM.main(HeapOOM.java:18)

 ```
#第三章