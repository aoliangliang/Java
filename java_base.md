# Java 基础总结
## 基础数据类型：
java中的基础（原生）类型有8种：byte,short,char,int,float,double,long,boolean<br/>
java中整形都是有符号的。<br/>
值得注意的是byte和boolean是一个字节，short和char是两个字节，char的编码是Unicode，int和float是四个字节，long和double是八个字节。<br/>
类型大小是固定的与平台无关。<br/>
目前这些基本类型都有自己的包装类。byte,short,int,float,double,long都继承自Number。编译器会自动装箱或自动拆箱，会自动插入指令而虚拟机不知道。<br/>
## 字符串
String底层使用char[]存储，Unicode编码。所有要改变字符串状态的方法都通过创建新的字符串实例返回，不会改变字符串底层的数据，即状态不可改变，线程安全。
String使用final修饰，即不可被继承。如果要平凡修改String状态，使用StringBuilder，主要有append方法。String比较有用的方法：<br/>
substring(int beginIndex,int endIndex)：获得两个索引之间的子串，不包含endIndex<br/>
charAt(int index)：某个位置的char<br/>
compareTo(String other) :实现了Comparable<String>接口<br/>
```java
import java.util.Scanner;
public class Demo01 {
public static void main(String[] args) {
     Scanner sc = new Scanner(System.in);
     for (;;) {
             String a = sc.next();
             String b = sc.next();
             int num = a.compareTo(b);
             System.out.println(num);
      }
   }
}
1.当两个比较的字符串是英文且长度不等时，
1）长度短的与长度长的字符一样，则返回的结果是两个长度相减的值
a="hello";
b="hell";
num=1;
或者
a="h";
b="hello";
num=4;
2)长度不一样且前几个字符也不一样,从第一位开始找，当找到不一样的字符时，则返回的值是这两个字符比较的值
a="assdf";
b="bdd";
num=-1;
2.当两个比较的字符串是英文且长度相等时，
1)一个字符
a="a";   //97
b="b";   //98
num=-1;
2）多个字符,第一个字符不同则直接比较第一个字符
a="ah";    //a=97
b="eg";    //e=101
num=-4;
3）多个字符,第一个字符相同则直接比较第二个字符,以此类推
a="ae";   //e=101
b="aa";   //a=97
num=4;
```
indexOf(String str)：子串首次出现的位置<br/>
trim():去除字符串前后的空白<br/>
toLowerCase()<br/>
toUpperCase()<br/>
静态方法：<br/>
String.format(String fmt, Object… ) :格式化字符串，后跟不定参数<br/>
```java
String str = "hello world"
String format = "you are %s";  
System.out.println(String.format(format, str)); // you are hello world  
```
[java字符串正则表达式语法和应用](http://www.runoob.com/java/java-regular-expressions.html)
## 标准输入和格式化输出
java.util.Scanner提供了标准输入，比较有用的方法有
Scanner(InputStream input) :初始化一个Scanner实例
String nextLine() : 读入一行，不论是否有空格
String hasNext() :是否还有字符串
String next() :以空格作为分隔符
String hasNextInt()
String nextInt() :读入一个整数
String hasNextDouble()
String nextDouble()：读入一个double
标准输入：new Scanner(System.in)
从文件输入：new Scanner(new File(filename))
格式化输出到控制台：System.out.printf(fmt, object...)
格式化输出到文件：PrintWriter(new File(filename))
printer.printf(fmt, object...)
printer.close()
```java
/* 
 * 格式化输出到文件 
 */  
String outName = "out.txt"; // 保存hello : world  
File outFile = new File(outName);  
System.out.println(outFile.getAbsolutePath());  
PrintWriter printer = new PrintWriter(outFile); // FileNotFoundException  
printer.printf("%s : %s\n", "hello", "world");  
printer.close();  
```
## 数组
数组创建元素是默认值，数字是0，对象是null，布尔是false。也可以在数组创建的时初始化，但是不能确定数组大小，数组的大小由初始化个数决定。
数组是对象，有length属性，可以赋值给另一个数组对象，数组对应的工具类Arrays。<br/>
数组转化为字符串：<br/>
Arrays.toString(type[] arr)<br/>
二维组快速输出：<br/>
Arrays.deepToString(type[][] arr)<br/>
数组拷贝：<br/>
newArr= Arrays.copyOf(type[] arr, int len) :不管数组大小，生成新的数组，与原来数组没有任何关系。<br/>
数组排序：<br/>
Arrays.sort(type[] arr)<br/>
数组二分查找，检查是否有该元素，没有则返回-1：<br/>
Arrays.binarySearch(type[] arr, int v)<br/>
数组整体赋值：<br/>
Arrays.fill(type[] arr, type v)<br/>
## 类
因为对象在堆中创建，类的[成员变量](http://blog.csdn.net/du_minchao/article/details/48881637)默认有初始值。而局部变量一般在栈中，在操作前必须明确
初始化，否则编译会报错。java中的类是要加上包名的，包名+类名构成的全名才是类名称。当存在类冲突时使用全名即可。包之间没有关系。类有三个特性：封装、继承、
多态。封装最主要体现在权限上，java中有四种权限private，default，protected，public。其中修饰类只能是public或者default。
private只能在本类可见，default是包权限，protected是包权限+子类权限（子类成员方法可以直接使用，而静态方法中可以通过生成子类的实例来访问，
但不可以使用父类实例来访问，见下面代码），public是所有权限。继承的话会继承所有成员，包括私有成员，当然子类可以有和父类同名成员，
但是父类的成员会被隐藏。子类可以覆盖父类的方法。多态主要体现在覆盖上。覆盖方法时权限不能缩小，抛出的异常不能增多，
子类中的返回值可以是父类返回值的子类型，其他如形参都必须保持一致。
多态的实现机制是每个类维护一张方法表，如果有覆盖，则表里的方法被替换掉。
具体要看引用指向的类型，这一点非常重要，赋值给的类型只是在操作时有些方法可能不能调用。<br/>
Object是所有类的超类，也就是所有类都继承Object的方法:equals，toString，hashCode，clone。
equals默认是比较变量的地址(实际编程时所有变量的状态相同，就可以认为两个实例相等)。
equals相等时，则hashCode必须相等。toString默认返回类名@哈希值。clone是protected权限，必须在子类扩大为public权限，这样才能实现对象拷贝。
对于数组整体输出，可以使用Arrays.toString()，但是集合类可以直接输出，这两个都要求具体类覆盖其toString()方法。<br/>
```java
public class Object {  
    private static native void registerNatives();  
    static {  
        registerNatives();  
    }  
    public native int hashCode();  
  
    public boolean equals(Object obj) {  
        return (this == obj);  
    }  
    protected native Object clone() throws CloneNotSupportedException;  
  
    public final native Class<?> getClass();  
  
    public String toString() {  
        return getClass().getName() + "@" + Integer.toHexString(hashCode());  
    }  
} 
```
有意思的是Object类中[native关键字](https://www.cnblogs.com/b3051/p/7484501.html)说明其修饰的方法是一个原生态方法，方法对应的实现不是在当前文件，
而是在用其他语言（如C和C++）实现的文件中。Java语言本身不能对操作系统底层进行访问和操作，但是可以通过JNI接口调用其他语言来实现对底层的访问。
JNI是Java本机接口（Java Native Interface），是一个本机编程接口，它是Java软件开发工具箱（java Software Development Kit，SDK）的一部分。
JNI允许Java代码使用以其他语言编写的代码和代码库。Invocation API（JNI的一部分）可以用来将Java虚拟机（JVM）嵌入到本机应用程序中，
从而允许程序员从本机代码内部调用Java代码。<br/>
native用法：<br/>
1.编写带有native声明的方法的Java类（java文件）<br/>
2.使用javac命令编译编写的Java类（class文件）<br/>
3.使用javah -jni ****来生成后缀名为.h的头文件（.h的文件）<br/>
4.使用其他语言（C、C++）实现本地方法<br/>
5.将本地方法编写的文件生成动态链接库（dll文件）<br/>
final关键字<br/>
final修饰的类不能被继承，修饰的方法不能被覆盖。对于全局成员，final必须在定义时初始化，与默认值和static无关。
而对于方法中的final，在操作前必须初始化。final修饰的变量只能被赋值一次。final的语义其实就是不可以改变引用的指向，但其指向的对象的状态仍然可以改变。
这点与C++的顶层const类似。状态不改变的类称为不可变类。注意两者的区别。
```java
package knowledge;  
  
/** 
 * Created by gzx on 16-12-29. 
 */  
// final类，不可以继承，所有方法均为final  
public final class FinalDemo {  
    // 全局final成员必须初始化  
    private final static int finalData = 1;  
    private int data;  
    public void setData(final int data){  
        // 传值时已经初始化了，不可以再改变data  
        // data = 2;  
        this.data = data;  
    }  
    // 不可以覆盖  
    public final int getData(){  
        return data;  
    }  
    // 方法中的final局部变量  
    public static void main(String[] args){  
        final int hello;  
       // System.out.println(hello); // 只要不操作hello，就可以不用初始化，与局部变量类似  
        final FinalDemo finalDemo = new FinalDemo();  
        // 只能赋值一次，但可以改变对象的状态  
       // finalDemo = new FinalDemo();  
        int data = 2;  
        finalDemo.setData(data);  
        System.out.println(finalDemo.getData()); // 2  
    }  
}  
```
[类的初始化顺序](https://www.cnblogs.com/qiuyong/p/6407418.html?utm_source=itdadao&utm_medium=referral)<br/>
有几个原则。静态成员和成员属性一开始都有默认值，如0,false,null，这是在定义初始化之前的。
静态成员是类级别的，所以在类加载时就已经初始化了，先定义初始化，然后运行静态块，且只初始化一次。
数据成员定义时先初始化，然后才是构造函数。父类先于子类。某个实例是子类的实例，必然是父类的实例，这是instanceof的语义。
如果子类覆盖了父类的方法，然后在父类的构造函数中调用，这时调用的是子类的覆盖方法，
但是方法中有多个父子都有的同名的数据成员，则使用子类的，方法体在哪个类中，就使用哪个类的属性，即使存在隐藏，且与权限无关。<br/>
初始化的顺序：<br/>
父类静态数据成员<br/>
父类静态块<br/>
静态数据成员<br/>
静态数据块  // 上述四个在类第一次加载时初始化， 且只初始化一次，构造子类实例会自动加载父类静态<br/>
父类数据成员定义时初始化<br/>
父类构造函数 // 如果这时调用了子类的覆盖函数，则由于未初始化，为默认值<br/>
子类数据成员定义时初始化<br/>
子类构造函数<br/>

命令行编译和运行<br/>
javac 中的-d选项用于指定生成的类放在哪个文件夹下，并在该文件夹下生成包目录。
如果不指定，则class文件与java源码文件在同一个目录，不会生成包目录。javac和java都有-cp选项。这个选项用于指定class字节码文件的目录
（这个目录下有完整的类包目录），或者指定对应的jar库（必须指定到具体的jar，不能只指定到jar所在的目录）。
当然，这个选项可以通过设置一个环境变量CLASSPATH替代，对于Linux，分隔符为:。
有一个区别，javac会自动查找当前目录，而java则只认定指定的cp选项，没有设置当前目录(.)，则可能会出错。
-cp使得在任何目录下都可以运行java程序。javac编译的文件以.java为后缀。java 运行的类必须给出是全名，且有正确的main函数，类是public。<br/>
编译：javac -d bin -cp lib/guava-17.0.jar src/a/A.java src/b/B.java  <br/>
运行：java -cp bin:lib/guava-17.0.jar a.A  <br/>
抽象类和接口 <br/>
抽象类，主要是类中有实现不了的抽象方法，加abstract，不用写出方法体。
或者所有方法都实现了，还可以添加abstract关键字，形成抽象类，希望被扩展。
接口有public接口和default接口，成员属性默认全部是public final static，方法全部是public。
接口主要是使得实现类符合某一规范，如要进行排序，则必须实现Comparable接口，或者提供Comparator比较器。
有些接口没有任何内容，称为标记接口。接口之间可以存在继承关系，用extends，表示扩展一个接口。
不实现抽象方法或者接口的方法，则还是抽象类。抽象类不可以实例化，但可以用匿名类实例化。 <br/>
```java
import java.io.*;  
import java.util.HashMap;  
import java.util.Map;  
  
/** 
 * Created by jessin on 17-2-26. 
 */  
abstract class AbstractLineProcessor {  
    protected abstract void processLine(String line);  
    public abstract int getAns();  
    public void processFile(String filename){  
        BufferedReader bf = null;  
        try {  
             bf = new BufferedReader(new FileReader(filename));  
            String line = null;  
            while((line = bf.readLine()) != null){  
                processLine(line);  
            }  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
        finally{  
            if(bf != null){  
                try {  
                    bf.close();  
                } catch (IOException e) {  
                    e.printStackTrace();  
                }  
            }  
        }  
    }  
}  
class NumberLineProcessor extends AbstractLineProcessor{  
    private int ans;  
    protected void processLine(String line) {  
        for(char aChar : line.toCharArray()){  
            if(Character.isDigit(aChar)){  
                ans++;  
            }  
        }  
    }  
  
    public int getAns() {  
        return ans;  
    }  
}  
  
class LetterLineProcessor extends AbstractLineProcessor{  
    private int ans;  
    protected void processLine(String line) {  
        for(char aChar : line.toCharArray()){  
            if(aChar >= 'a' && aChar <= 'z' || aChar >= 'A' && aChar <= 'Z'){  
                ans++;  
            }  
        }  
    }  
  
    public int getAns() {  
        return ans;  
    }  
}  
public class Main{  
    public static void main(String[] args){  
        final Map<String, AbstractLineProcessor> map = new HashMap<String, AbstractLineProcessor>();  
        map.put("number", new NumberLineProcessor());  
        map.put("letter", new LetterLineProcessor());  
  
        // 匿名内部类，实现对一行进行所有的行处理，最后得到结果  
        AbstractLineProcessor all = new AbstractLineProcessor(){  //匿名实例化
        
            protected void processLine(String line) {  
                for(AbstractLineProcessor lineProcessor : map.values()){  
                    lineProcessor.processLine(line);  
                }  
            }  
  
            public int getAns() {  
                return 0;  
            }  
        };  
        all.processFile("hello.txt");  
        for(AbstractLineProcessor lineProcessor : map.values()){  
            System.out.println(lineProcessor.getAns());  
        }  
    }  
}  
```










