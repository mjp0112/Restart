## File类

java.io.FIle类

File类是java.io包下代表与平台无关的文件和目录，也就是说如果希望在程序中操作文件和目录都可以通过File类来完成，File类能新建、删除、重命名文件和目录。



### 1.构造方法

* public File(String pathname) ：通过将给定的路径名字符串转换为抽象路径名来创建新的 File实例。  
* public File(String parent, String child) ：从父路径名字符串和子路径名字符串创建新的 File实例。
* public File(File parent, String child) ：从父抽象路径名和子路径名字符串创建新的 File实例。  



### 2.常用方法

#### 2.1.获取基本信息

```java
//此File表示的文件或目录的名称。  
String getName();
//返回由此File表示的文件的长度。 
long length();


String getPath()  //将此File转换为路径名字符串。 
String getParent() //获得父级路径
String getAbsolutePath() //返回此File的绝对路径名字符串。

```

#### 2.2.判断

- `public boolean exists()` ：此File表示的文件或目录是否实际存在。
- `public boolean isDirectory()` ：此File表示的是否为目录。
- `public boolean isFile()` ：此File表示的是否为文件。
- `public isAbsolute()`：判断File对象对应的文件或目录是否是绝对路径
- `public boolean canRead()`：判断File对象对应的文件或目录是否可读
- `public boolean canWrite()`：判断File对象对应的文件或目录是否可写
- `public boolean isHidden()`：判断File对象对应的文件或目录是否是否隐藏



#### 2.3.创建删除

```java
public boolean createNewFile() 当且仅当具有该名称的文件尚不存在时，创建一个新的空文件
    并且当前目录要存在。
    
public boolean delete() 删除由此File表示的文件或目录，只能删除空目录。
    
public boolean mkdir() 创建由此File表示的目录。
    
public boolean mkdirs() 创建由此File表示的目录，包括任何必需但不存在的父目录。
```





#### 2.4.目录遍历

```java
//获取当前目录下的文件以及文件夹的名称。
String[] names = dir.list();

//获取当前目录下的文件以及文件夹对象
 File[] files = dir.listFiles();
```





### 3.递归打印文件

```java
public void listSubFiles(File dir) {
		if (dir != null && dir.isDirectory()) {
			File[] listFiles = dir.listFiles();
			if (listFiles != null) {
				for (File sub : listFiles) {
					listSubFiles(sub);//递归调用
				}
			}
		}
		System.out.println(dir);
	}
```





## IO概述

数据的传输，可以看做是一种数据的流动，按照流动的方向，以内存为基准，分为`输入input` 和`输出output` ，即流向内存是输入流，流出内存的输出流。

Java中I/O操作主要是指使用`java.io`包下的内容，进行输入、输出操作。**输入**也叫做**读取**数据，**输出**也叫做作**写出**数据。



### 1.IO分类

#### 1.1 输入和输出流

根据数据的流向分类

- **输入流** ：把数据从`其他设备`上读取到`内存`中的，读操作

  - 以InputStream,Reader结尾

- **输出流** ：把数据从`内存` 中写出到`其他设备`上的流，写操作

  - 以OutputStream、Writer结尾

    

#### 1.2 字节流和字符流

根据数据类型

- **字节流** ：以字节为单位，读写数据的流，比如读取图片；
  - 以InputStream和OutputStream结尾
- **字符流** ：以字符为单位，读写数据的流，比如文本。
  - 以Reader和Writer结尾



### 2.字节流

一切文件数据(文本、图片、视频等)在存储时，都是以二进制数字的形式保存，都一个一个的字节，那么传输时一样如此。所以，字节流可以传输任意文件数据。在操作流的时候，我们要时刻明确，无论使用什么样的流对象，底层传输的始终为二进制数据。



#### 2.1 字节输出流OutputStream

- `public void close()` ：关闭此输出流并释放与此流相关联的任何系统资源。  
- `public void flush() ` ：刷新此输出流并强制任何缓冲的输出字节被写出。  
- `public void write(byte[] b)`：将 b.length字节从指定的字节数组写入此输出流。  
- `public void write(byte[] b, int off, int len)` ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流。  
- `public abstract void write(int b)` ：将指定的字节输出流。



#### 2.2 子类FileOutputStream

##### 2.2.1构造方法

- `public FileOutputStream(File file)`：创建文件输出流以写入由指定的 File对象表示的文件。 
- `public FileOutputStream(String name)`： 创建文件输出流以指定的名称写入文件。  

当你创建一个流对象时，必须传入一个文件路径。该路径下，如果没有这个文件，会创建该文件。如果有这个文件，会清空这个文件的数据。



##### 2.2.2常用方法

```java
public class FOSWrite {
    public static void main(String[] args) throws IOException {
        // 使用文件名称创建流对象
        FileOutputStream fos = new FileOutputStream("fos.txt");     
      	
        // 写出字节
      	fos.write(97); // 写出第1个字节 a
      	fos.write(98); // 写出第2个字节 b
      	fos.write(99); // 写出第3个字节 c
        //结果为abc
      	
        
        // 字符串转换为字节数组
      	byte[] b = "尚硅谷".getBytes();
      	// 写出字节数组数据
      	fos.write(b);
        
        // 字符串转换为字节数组
      	byte[] b = "abcde".getBytes();
		// 写出从索引2开始，2个字节。索引2是c，两个字节，也就是cd。
        fos.write(b,2,2);
        
        
      	// 关闭资源
        fos.close();
        
    }
}
```

- 回车符`\r`和换行符`\n` ：
  - 回车符：回到一行的开头（return）。
  - 换行符：下一行（newline）。
- 系统中的换行：
  - Windows系统里，每行结尾是 `回车+换行` ，即`\r\n`；
  - Unix系统里，每行结尾只有 `换行` ，即`\n`；
  - Mac系统里，每行结尾是 `回车` ，即`\r`。从 Mac OS X开始与Linux统一。



##### 2.2.3数据追加

以普通方式构造默认会清空现有文件，可以选择追加。

- `public FileOutputStream(File file, boolean append)`： 创建文件输出流以写入由指定的 File对象表示的文件。  
- `public FileOutputStream(String name, boolean append)`： 创建文件输出流以指定的名称写入文件。  





#### 2.3 字节输入流 InputStream

java.io.InputStream 抽象类是表示字节输入流的所有类的超类，可以读取字节信息到内存中。它定义了字节输入流的基本共性功能方法。

- `public void close()` ：关闭此输入流并释放与此流相关联的任何系统资源。    
- `public abstract int read()`： 从输入流读取数据的下一个字节。 
- `public int read(byte[] b)`： 从输入流中读取一些字节数，并将它们存储到字节数组 b中 。



#### 2.4 FileInputStream类

java.io.FileInputStream 类是文件输入流，从文件中读取字节。

- `FileInputStream(File file)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的 File对象 file命名。 
- `FileInputStream(String name)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name命名。  



##### 2.4.1 常用方法

**读取字节**

char数组在系统中是以一字节存储的，fileinputstream读取时，提升为int类型；读取到末尾，返回-1.

```java
		// 使用文件名称创建流对象
       	FileInputStream fis = new FileInputStream("read.txt");
      	// 读取数据，返回一个字节
        int read = fis.read();
        System.out.println((char) read);

		fis.close();
```

改进为循环读取

```java
while((b=fis.read())!=-1){
    System.out.println((char)b);
}
```



**使用字节数组读取**

每次读取b的长度个字节到数组中，返回读取到的有效字节个数，读取到末尾时，返回`-1

==以下这种会错误，因为如果最后一次没读取满整个数组，会保留有倒数第二次的数据==

```java
		int len ;
        // 定义字节数组，作为装字节数据的容器   
        byte[] b = new byte[2];
        // 循环读取
        while (( len= fis.read(b))!=-1) {
           	// 每次读取后,把数组变成字符串打印
           //这里会可能出现错误
            System.out.println(new String(b));
        }
		// 关闭资源
        fis.close();
```

--改进--

```java
        int len ；
        // 定义字节数组，作为装字节数据的容器   
        byte[] b = new byte[2];
        // 循环读取
        while (( len= fis.read(b))!=-1) {
           	// 每次读取后,把数组的有效字节部分，变成字符串打印
            System.out.println(new String(b，0，len));//  len 每次读取的有效字节个数
        }
		// 关闭资源
        fis.close();
```



#### 2.5 读取图片

```java
		FileInputStream fis = new FileInputStream("D:\\1.jpg");
        FileOutputStream fos =new FileOutputStream("D:\\2.jpg");
        int len;
        byte[] b = new byte[1024];
        while ((len=fis.read(b))!=-1){
            fos.write(b,0,len);
        }
        fos.close();
        fis.close();
```



### 3. 字符流

#### 3.1 FileReader类  

`java.io.FileReader `类是读取字符文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

> 小贴士：
>
> 1. 字符编码：字节与字符的对应规则。Windows系统的中文编码默认是GBK编码表。
>
> eclipse中默认GBK，idea中默认UTF-8
>
> 2. 字节缓冲区：一个字节数组，用来临时存储字节数据。

##### 3.1.1 构造方法

- `FileReader(File file)`： 创建一个新的 FileReader ，给定要读取的File对象。   

- `FileReader(String fileName)`： 创建一个新的 FileReader ，给定要读取的文件的名称。

  

##### 3.1.2 常用方法

**读取字符：**`read`方法，每次可以读取一个字符的数据，提升为int类型，读取到文件末尾，返回`-1`，循环读取；

```java
		// 定义变量，保存数据
        int b ；
        // 循环读取
        while ((b = fr.read())!=-1) {
            System.out.println((char)b);
        }
```

**读取字符组：**

```java
		int len ;
        // 定义字符数组，作为装字符数据的容器
        char[] cbuf = new char[2];
        // 循环读取
        while ((len = fr.read(cbuf))!=-1) {
            System.out.println(new String(cbuf,0,len));
        }
```



#### 3.2 FileWriter

`java.io.FileWriter `类是写出字符到文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

##### 3.2.1 构造方法

- `FileWriter(File file)`： 创建一个新的 FileWriter，给定要读取的File对象。   
- `FileWriter(String fileName)`： 创建一个新的 FileWriter，给定要读取的文件的名称。  



##### 3.2.2 常用方法

**写出字符**

```java
      	fw.write(97); // 写出第1个字符
      	fw.write('b'); // 写出第2个字符
      	fw.write('C'); // 写出第3个字符
      	fw.write(30000); // 写出第4个字符，中文编码表中30000对应一个汉字。
```

==小贴士：==

1. 虽然参数为int类型四个字节，但是只会保留一个字符的信息写出。
2. 未调用close方法，数据只是保存到了缓冲区，并未写出到文件中。



**关闭和刷新**

因为内置缓冲区的原因，如果不关闭输出流，无法写出字符到文件中。但是关闭的流对象，是无法继续写出数据的。如果我们既想写出数据，又想继续使用流，就需要`flush` 方法了。

- `flush` ：刷新缓冲区，流对象可以继续使用。
- `close `:先刷新缓冲区，然后通知系统释放资源。流对象不可以再被使用了。



**其他方法**

1. **写出字符数组** ：`write(char[] cbuf)` 和 `write(char[] cbuf, int off, int len)` ，每次可以写出字符数组中的数据，用法类似FileOutputStream；
2. **写出字符串**：`write(String str)` 和 `write(String str, int off, int len)` ，每次可以写出字符串中的数据；
3. **续写和换行**：操作类似于FileOutputStream。



### 4.缓冲流

缓冲流,也叫高效流，按照数据类型分类：

* **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream` 
* **字符缓冲流**：`BufferedReader`，`BufferedWriter`

缓冲流的基本原理，是在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统IO次数，从而提高读写的效率。



#### 4.1 字节缓冲流

##### 4.1.1构造方法

```java
// 创建字节缓冲输入流
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("bis.txt"));
// 创建字节缓冲输出流
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("bos.txt"));
```

使用方法，与普通的字节输入输出流一样；



#### 4.2 字符缓冲流

##### 4.2.1 构造方法

```java
// 创建字符缓冲输入流
BufferedReader br = new BufferedReader(new FileReader("br.txt"));
// 创建字符缓冲输出流
BufferedWriter bw = new BufferedWriter(new FileWriter("bw.txt"));
```

字符缓冲流的基本方法与普通字符流调用方式一致，我们来看它们具备的特有方法。

* BufferedReader：`public String readLine()`: 读一行文字。 
* BufferedWriter：`public void newLine()`: 写一行行分隔符,由系统属性定义符号。 

```java
public class BufferedReaderDemo {
    public static void main(String[] args) throws IOException {
      	 // 创建流对象
        BufferedReader br = new BufferedReader(new FileReader("in.txt"));
		// 定义字符串,保存读取的一行文字
        String line  = null;
      	// 循环读取,读取到最后返回null
        while ((line = br.readLine())!=null) {
            System.out.print(line);
            System.out.println("------");
        }
        
        BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"));
      	// 写出数据
        bw.write("尚");
      	// 写出换行
        bw.newLine();
        
		// 释放资源
        br.close();
        bw.close();
    }
}


```



### 5.转换流

字符集：IDEA是UTF-8，而windows默认GBK；



#### 5.1 InputStreamReader类

转换流`java.io.InputStreamReader`，是Reader的子类，是从字节流到字符流的桥梁。它读取字节，并使用指定的字符集将其解码为字符。它的字符集可以由名称指定，也可以接受平台的默认字符集。 

##### 构造方法

* `InputStreamReader(InputStream in)`: 创建一个使用默认字符集的字符流。 
* `InputStreamReader(InputStream in, String charsetName)`: 创建一个指定字符集的字符流。



#### 5.2OutputStreamWriter类

转换流`java.io.OutputStreamWriter` ，是Writer的子类，是从字符流到字节流的桥梁。使用指定的字符集将字符编码为字节。它的字符集可以由名称指定，也可以接受平台的默认字符集。 

##### 构造方法

- `OutputStreamWriter(OutputStream in)`: 创建一个使用默认字符集的字符流。 
- `OutputStreamWriter(OutputStream in, String charsetName)`: 创建一个指定字符集的字符流。





### 6.数据流

前面学习的IO流，在程序代码中，要么将数据直接按照字节处理，要么按照字符处理。那么，如果要在程序中直接处理Java的基础数据类型，怎么办呢？

```java
String name = “巫师”;
int age = 300;
char gender = ‘男’;
int energy = 5000;
double price = 75.5;
boolean relive = true;
```

完成这个需求，可以使用DataOutputStream进行写，随后用DataInputStream进行读取，而且顺序要一致。

示例代码：

```java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class TestData {
	public void save() throws IOException{
		String name = "巫师";
		int age = 300;
		char gender = '男';
		int energy = 5000;
		double price = 75.5;
		boolean relive = true;
		
		DataOutputStream dos = new DataOutputStream(new FileOutputStream("game.dat"));
		dos.writeUTF(name);
		dos.writeInt(age);
		dos.writeChar(gender);
		dos.writeInt(energy);
		dos.writeDouble(price);
		dos.writeBoolean(relive);
		dos.close();
	}	
	public void reload()throws IOException{
		DataInputStream dis = new DataInputStream(new FileInputStream("game.dat"));
		String name = dis.readUTF();
		int age = dis.readInt();
		char gender = dis.readChar();
		int energy = dis.readInt();
		double price = dis.readDouble();
		boolean relive = dis.readBoolean();
		
		System.out.println(name+"," + age + "," + gender + "," + energy + "," + price + "," + relive);
		
		dis.close();
	}
}
```



### 7.序列化

Java 提供了一种对象**序列化**的机制。用一个字节序列可以表示一个对象，该字节序列包含该`对象的类型`和`对象中存储的属性`等信息。字节序列写出到文件之后，相当于文件中**持久保存**了一个对象的信息。 

反之，该字节序列还可以从文件中读取回来，重构对象，对它进行**反序列化**。`对象的数据`、`对象的类型`和`对象中存储的数据`信息，都可以用来在内存中创建对象。看图理解序列化： 

![](JAVA28文件和IO.assets/3_xuliehua.jpg)





#### 序列化操作

* 该类必须实现`java.io.Serializable ` 接口，`Serializable` 是一个标记接口，不实现此接口的类将不会使任何状态序列化或反序列化，会抛出`NotSerializableException` 。
  * 如果对象的某个属性也是引用数据类型，那么如果该属性也要序列化的话，也要实现`Serializable` 接口
* 该类的所有属性必须是可序列化的。如果有一个属性不需要可序列化的，则该属性必须注明是瞬态的，使用`transient` 关键字修饰。
* 静态变量的值不会序列化

```java
public class Employee implements java.io.Serializable {
    public static String company = "尚硅谷";
    public String name;
    public String address;
    public transient int age; // transient瞬态修饰成员,不会被序列化
    public void addressCheck() {
      	System.out.println("Address  check : " + name + " -- " + address);
    }
}
```

2.写出对象方法

* `public final void writeObject (Object obj)` : 将指定的对象写出。

```java
public class SerializeDemo{
   	public static void main(String [] args)   {
    	Employee e = new Employee();
    	e.name = "zhangsan";
    	e.address = "beiqinglu";
    	e.age = 20; 
    	try {
      		// 创建序列化流对象
          ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("employee.txt"));
        	// 写出对象
        	out.writeObject(e);
        	// 释放资源
        	out.close();
        	fileOut.close();
        	System.out.println("Serialized data is saved"); // 姓名，地址被序列化，年龄没有被序列化。
        } catch(IOException i)   {
            i.printStackTrace();
        }
   	}
}
输出结果：
Serialized data is saved
```



#### 反序列化操作1

如果能找到一个对象的class文件，我们可以进行反序列化操作，调用`ObjectInputStream`读取对象的方法：

- `public final Object readObject ()` : 读取一个对象。

```java
public class DeserializeDemo {
   public static void main(String [] args)   {
        Employee e = null;
        try {		
             // 创建反序列化流
             FileInputStream fileIn = new FileInputStream("employee.txt");
             ObjectInputStream in = new ObjectInputStream(fileIn);
             // 读取一个对象
             e = (Employee) in.readObject();
             // 释放资源
             in.close();
             fileIn.close();
        }catch(IOException i) {
             // 捕获其他异常
             i.printStackTrace();
             return;
        }catch(ClassNotFoundException c)  {
        	// 捕获类找不到异常
             System.out.println("Employee class not found");
             c.printStackTrace();
             return;
        }
        // 无异常,直接打印输出
        System.out.println("Name: " + e.name);	// zhangsan
        System.out.println("Address: " + e.address); // beiqinglu
        System.out.println("age: " + e.age); // 0
    }
}
```

**对于JVM可以反序列化对象，它必须是能够找到class文件的类。如果找不到该类的class文件，则抛出一个 `ClassNotFoundException` 异常。**  

#### **反序列化操作2**

**另外，当JVM反序列化对象时，能找到class文件，但是class文件在序列化对象之后发生了修改，那么反序列化操作也会失败，抛出一个`InvalidClassException`异常。**发生这个异常的原因如下：

* 该类的序列版本号与从流中读取的类描述符的版本号不匹配 
* 该类包含未知数据类型  

`Serializable` 接口给需要序列化的类，提供了一个序列版本号。`serialVersionUID` 该版本号的目的在于验证序列化的对象和对应类是否版本匹配。

```java
public class Employee implements java.io.Serializable {
     // 加入序列版本号
     private static final long serialVersionUID = 1L;
     public String name;
     public String address;
     // 添加新的属性 ,重新编译, 可以反序列化,该属性赋为默认值.
     public int eid; 

     public void addressCheck() {
         System.out.println("Address  check : " + name + " -- " + address);
     }
}
```

### 

