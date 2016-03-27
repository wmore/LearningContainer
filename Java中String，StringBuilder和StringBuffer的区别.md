##**Java中String，StringBuilder和StringBuffer的区别**

###2016-03-27

-----

发现别人代码里会使用StringBuilder来操作字符串，这是什么东西，看起来方法和StringBuffer差不多，以前我都是用StringBuffer的。上网查了下这俩者的区别。

引用自[http://blog.csdn.net/huaye502/article/details/6603592][1]

Java对象的引用，用来操纵内存元素。例如：String s;//创建一个String类型的引用
对象的引用与对象关联——初始化，例如：s = "abc";//Java语言特性，String可以用带引号的文本初始化。
更通用的初始化，创建对象，例如：s = new String("abc");——《Thinking in Java》

StringBuilder类，对应于String类，StringBuilder具有可变性。
StringBuilder是Java SE5引入的，线程非安全的。
StringBuffer是Java SE5之前就有的线程安全的，相对来说较StringBuilder开销稍大
两者的常用操作：append(),toString(),insert(),reverse(),delete()等。

String：不可变的对象，对String对象进行改变的时候其实都等同于生成了一个新的String对象，然后将引用指向新的String对象，原String对象GC回收。
StringBuffer 字符串变量（线程安全），适用于多线程程序中，保证同步性。
StringBuilder 字符串变量（非线程安全），适用于单线程程序中，不保证同步性。简要的说， String 类和 StringBuffer/StringBuilder 类的主要性能区别其实在于 String 是不可变的对象, 因此在每次对 String 类进行改变的   时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象，所以经常改变内容的字符串最好不要用 String ，因为每次生成   对象都会对系统性能产生影响，特别当内存中无引用对象多了以后， JVM 的 GC 就会开始工作，那速度是一定会相当慢的。   而如果是使用 StringBuffer/StringBuilder 类则结果就不一样了，每次结果都会对 StringBuffer/StringBuilder 对象本身进行操作，而不是生成   新的对象，再改变对象引用。所以在一般情况下推荐使用 StringBuffer/StringBuilder ，特别是字符串对象经常改变的情况下。而在某些特别情况下，   String 对象的字符串拼接其实是被 JVM 解释成 StringBuffer/StringBuilder 对象的拼接，所以这些时候 String 对象的速度并不会比  StringBuffer/StringBuilder 对象慢，而特别是以下的字符串对象 生成中， String 效率是远要比 StringBuffer 快的：

   `String S1 = “This is only a” + “ simple” + “ test”;`
   `StringBuilder Sb = new StringBuilder(“This is only a”).append(“ simple”).append(“ test”);`
   你会很惊讶的发现，生成 String S1 对象的速度简直太快了，而这个时候 StringBuffer 居然速度上根本一点都不占优势。其实这是 JVM 的一个把戏，
   在 JVM 眼里，这个
   String S1 = “This is only a” + “ simple” + “test”; 其实就是：
   String S1 = “This is only a simple test”; 所以当然不需要太多的时间了。但大家这里要注意的是，如果你的字符串是来自另外的 String 对象
   的话，速度就没那么快了，譬如：
   `String S2 = “This is only a”;`
   String S3 = “ simple”;
   String S4 = “ test”;
   `String S1 = S2 +S3 + S4;`
   这时候 JVM 会规规矩矩的按照原来的方式去做


在大部分情况下 StringBuffer > String
StringBuffer
Java.lang.StringBuffer线程安全的可变字符序列。一个类似于 String 的字符串缓冲区，但可以修改。虽然在任意时间点上它都包含某
种特定的字符序列，但通过某些方法调用可以改变该序列的长度和内容。可将字符串缓冲区安全地用于多个线程。可以在必要时对这些方法进行同步，因此任意特定实例上的所有操作就好像是以串行顺序发生的，该顺序与所涉及的每个线程进行的方法调用顺序一致。StringBuffer 上的主要操作是 append 和 insert 方法，可重载这些方法，以接受任意类型的数据。每个方法都能有效地将给定的数据转换成字符串，然后将该字符串的字符追加或插入到字符串缓冲区中。append 方法始终将这些字符添加到缓冲区的末端；而 insert 方法则在指定的点添加字符。例如，如果 z 引用一个当前内容是“start”的字符串缓冲区对象，则此方法调用 z.append("le") 会使字符串缓冲区包含“startle”，
而 z.insert(4, "le") 将更改字符串缓冲区，使之包含“starlet”。


在大部分情况下 StringBuilder > StringBuffer
java.lang.StringBuilder
java.lang.StringBuilder一个可变的字符序列是5.0新增的。此类提供一个与 StringBuffer 兼容的 API，但不保证同步。该类被设计用
作 StringBuffer 的一个简易替换，用在字符串缓冲区被单个线程使用的时候（这种情况很普遍）。如果可能，建议优先采用该类，因为在
大多数实现中，它比 StringBuffer 要快。两者的方法基本相同。




[1]: http://blog.csdn.net/huaye502/article/details/6603592