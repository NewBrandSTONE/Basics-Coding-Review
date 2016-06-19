# IO 复习
##File
---

* 构造方法

```java
File(String pathName);
// 通过将给定路径名字字符串转为抽象路径来创建一个新的File实例。
File(File parent, String child);
// 根据parent抽象路径名和child路径名字符串创建一个新的File实例。
```

##File类的高级获取
---

* 常用方法

```java
String list();
// 返回一个字符串数组，这些字符串指定抽象路径名和文件
File listFiles();
// 返回一个抽象路径名数组，这些路径名表示抽象路径名中的文件
```

* 分析过程

```java
private static void searchFiles(File file) {
    if (file.isDirectory()) {
        File[] files = file.listFiles();
        if (files != null) {
            for (File f : files) {
                if (f.isFile) {
                    sysout(f);
                } else {
                    // 重复上面的逻辑
                    searchFiles(f);
                }
            }
        }
    }
}
```
##FileReader
---

* 专门读取字符数据的

```java
public class FileReader extends InputStreamReader
```

* 构造方法

```java
FileReader(String fileName)
// 在给定从中去读数据的文件名的情况下创建一个新的FileReader
```

* 常用方法

```java
int read(); // 读取单个字符。
// 读取的字符数，如果已到达流的末尾，则返回 -1
int read(char[] cbuf); // 将字符读入数组。
// 读取的字符数，如果已到达流的末尾，则返回 -1
```

* 示例代码


```java
Reader reader = new FileReader("c:/1.txt");
char[] buf = new char[5];
// 读取数据到缓冲数组中
int len;
while ((len = reader.read(buf)) != -1) {
    sysout(new String(buf, 0, len));
}
```
##FileWriter
---
* 专门往文件中写字符数据的


```java
public class FileWriter extends OutputStreamWriter
```

* 构造方法


```java
FileWriter(String fileName); // 根据给定的文件名构造一个FileWriter对象。
FileWriter(String fileName, boolean append);// 根据给定的文件名以及指示是否附加写入数据的boolean值来构造FileWriter对象。
```

* 常用方法


```java
void write(String str); //写入字符串。
abstract void write(char[] cbuf, int off, int len) // 写入字符数组的某一部分
```

* 分析过程


```java
// 写数据
writer.write("我是大魔王");
writer.flush();
// 字符流底层都是有缓冲区的，当我们调用写的方法的时候，是把数据写入底层的缓冲区
```

* 示例代码


```java
// 写字符数据
Writer writer = new FileWriter("c:/1.txt");
// 写操作
writer.write("我是大魔王");
// 字符流的写是写入底层的缓冲区，要把数据写到文件上还需要刷新缓冲区
// 刷新缓冲区
writer.flush();// 没有刷新缓冲区，数据是不会写入文件的
```
##拷贝文本文件
---

* 字节流


```java
private static void byByteStream() throws Exception {
    InputStream is = new FileInputStream("c:/1.java");
    OutputStream out = new FileOutputStream("c:/2.java");
    int len;
    byte[] buf = new byte[1024];
    while ((len = is.read(buf)) != -1) {
        // 边读边写
        out.wirte(buf, 0, len);
    }
    // 关闭资源
    is.close();
    out.close();
}
```

* 字符流


```java
private static void byCharStream() throws Exception {
    Reader reader = new FileReader("c:/1.java");
    Writer writer = new FileWriter("c:/2.java");
    char[] cbuf = new char[1024];
    int len;
    while ((len = reader.read(buf)) != -1) {
        writer.write(cbuf, 0, len);
    }
    // 关闭资源
    reader.close();
    writer.close();
}
```
##字符的编码和解码
---
编码与解码之间的转换关系
>编码：字符-->字节
>解码：字节-->字符
他们之间的转换靠的是编码表

* String的编码方法


```java
byte[] getBytes();
// 使用平台的默认字符集将次String编码为byte序列，并将结果存储到一个新的byte数组中。
byte[] getBytes(String charsetName)
// 使用指定的字符集将次String编码为byte序列，并将结果存储到一个新的byte数组中。
```

* String的解码方式依靠构造方法


```java
String(byte[] bytes);
// 通过平台的默认字符集将指定的byte的数组，构造成一个新的String。
String(byte[] bytes, String charsetName)
// 通过使用指定的charset解码指定的byte数据，构造一个新的String。
```

##转换流
---
* OutputStreamReader--是字符流通向字节流的桥梁；

* InputStreamWriter--是字节流通向字符流的桥梁。


```java		
//准备好输出
OutputStream out = new FileOutputStream("c:/2.txt");
//把输出字节流转变成输出字符流
Writer writer = new OutputStreamWriter(out, "utf-8");
writer.write("嘿嘿嘿");
writer.close();

InputStream is = new FileInputStream("c:/2.txt");
Reader reader = new InputStreamReader(is, "utf-8");
int read =reader.read();
System.out.println((char) read);
read.close();
```
##字节缓存流
---

>用得最多的包装流就是缓存流，默认内部维护了一个8KB的缓存区(能够有效的减少IO次数，从而提高IO的操作效率)。


```java
BufferedInputStream is = new BufferedInputStream(new FileInputStream("c:/setup.exe"));
BufferedOutputStream out = new BufferedOutputStream(new FileOutputStream("c:/1.ext"));
int len;
byte[] buf = new byte[1024];
// 先读取数据，在写数据
while ((len = is.read(buf)) != -1) {
    out.write(buf, 0, len);
}
// 关闭资源
is.close();
out.close();
```
##字符缓冲流
---
`public class BufferedReader extends Reader`
 
`reader.readLine();`可以直接读取一行数据

底层同样是自带一个`8K`的缓冲区

从字符输入流中读取文本，缓冲各个字符，从而实现字符、数组和行的高效读取。


###构造方法
---
`BufferedReader(Reader in)`

* 创建一个使用默认大小输入缓冲区的缓冲字符输入流


```java
//常用方法
String readLine();// 读取一个文本行。
public class BufferedWriter extends Writer
//可以直接写换行，writer.newLine();
```
``



###构造方法
`BufferedWriter(Writer out)`
```java
// 常用方法
void newLine();// 写入一个行分隔符
```

示例代码
```java
// 字符缓冲流
BufferedReader reader = new BufferedReader(new FileReader("c:/1.java"));
BufferedWriter writer = new BufferedWriter(new FileWriter("c:/2.java"));
String data;
// 直接读取一行数据
while ((data = reader.readLine()) != null) {
    writer.write(data);
    // 写一个换行符
    writer.newLine();
}
reader.close();
writer.close();
```
##属性文件
`Properties`类表示了一个持久的属性集。

`Properties`可保存在流中或者从流中加载。

* 属性列表中每个键以及对应值都是一个字符串。

```java
public class Properties extends Hashtable<Object, Object>
// 这个类是专门用于操作属性文件properties的
```

* 为什么要使用文件属性？

>1.解除硬编码，源代码是要编译的。

>2.改善程序的结构，有利于日后的维护。

* 构造方法

`Properties();` 创建一个无默认值得空属性列表。

* 常用方法


```java
void load(InputStream inStream);
// 从流中读取属性列表（键和元素对）。
String getProperty(String key);
// 用指定的键在此属性列表中搜索属性。
```

* 属性文件的写法

```xml
src=c:/setup.exe
dest=c:/1.exe
```

* 示例代码
```java
// 使用Properties文件
Properties p = new Properties();
// 从流中去读取属性的配置信息
InputStream in = new FileInputStream("file.properties");
p.load(in);

// 获取属性的值
String src = p.getProperty("src");
InputStream is = new FileInputStream(src);
String dest = p.getProperty("dest");
OutputStream out = new FileOutputStream(dest);
```
##ObjectOutputStream
* 用途

>把内存中的对象输出到文件或者把文件中保存的对象输入到内存

* 目的

>为了进程之间的数据交流


* 构造方法

```java
ObjectOutputStream(OutputStream out);
// 创建写入指定OutputStream的ObjectOutputStream
```

* 重用方法

```java
void writeObject(Object obj);
// 将指定的对象写入ObjectOutputStream。
```

* 示例代码

```java
// 使用对象的输出流
ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("c:/1.txt"));
// 写数据
// 写出的内容不是给人看的，是给机器看的
User u = new User("罗杰斯", 108);
out.writeObject(u);
// 把内存中的对象写出到硬盘-->序列化
out.close();
```

* 对象要序列化必须实现序列化接口

```java
public class User implements Serializable {
    String name;
    int age;
}
```
##ObjectInputStream
---
###序列化与反序列化之间的关系
---
>把内存中的对象写入硬盘-->序列化

>把硬盘中的对象读入内存-->反序列化

 
###构造方法
---
```java
ObjectInputStream(InputStream in);
// 创建从指定InputStream读取的ObjectInpuStream对象。
```
##内存流
---

###ByteArrayOutputStream:



				

