[TOC]



## 环境配置

### Java

​    两个路径 

- JDK     D:/Java/Jdk1.8.112         Java Development Kit   java开发工具包
- JRE      D:/Java/Jre                      Java Runtime Enviroment   Java运行环境

​    后续web中可能会遇到问题，如果将jre装到jdk中

WIN + d  回到桌面

WIN + e  打开文件资源管理器

WIN + R  打开运行窗口

ctrl + alt + l	：格式化

win+e-》这台电脑/桌面上我的电脑  -》 右击 -》 左边有个高级系统设置-》环境变量

-》 系统变量-》新建-》变量名: ```JAVA_HOME```,变量值：JDK自己的安装路径（```C:\software\java\jdk1.8.0_202```）-》找到Path-》编辑-》右上角新建-》填入内容如下```%JAVA_HOME%\bin``` -》确定   

**测试**

win + r ->输入cmd-》出来窗口中输入  

**java**

```
C:\Users\86137>java
用法: java [-options] class [args...]
           (执行类)
   或  java [-options] -jar jarfile [args...]
           (执行 jar 文件)
其中选项包括:
    -d32          使用 32 位数据模型 (如果可用)
    -d64          使用 64 位数据模型 (如果可用)
    -server       选择 "server" VM
                  默认 VM 是 server.

    -cp <目录和 zip/jar 文件的类搜索路径>
    -classpath <目录和 zip/jar 文件的类搜索路径>
                  用 ; 分隔的目录, JAR 档案
                  和 ZIP 档案列表, 用于搜索类文件。
    -D<名称>=<值>
                  设置系统属性
    -verbose:[class|gc|jni]
                  启用详细输出
    -version      输出产品版本并退出
```

**javac**

```
C:\Users\86137>javac
用法: javac <options> <source files>
其中, 可能的选项包括:
  -g                         生成所有调试信息
  -g:none                    不生成任何调试信息
  -g:{lines,vars,source}     只生成某些调试信息
  -nowarn                    不生成任何警告
  -verbose                   输出有关编译器正在执行的操作的消息
  -deprecation               输出使用已过时的 API 的源位置
  -classpath <路径>            指定查找用户类文件和注释处理程序的位置
  -cp <路径>                   指定查找用户类文件和注释处理程序的位置
  -sourcepath <路径>           指定查找输入源文件的位置
  -bootclasspath <路径>        覆盖引导类文件的位置
  -extdirs <目录>              覆盖所安装扩展的位置
  -endorseddirs <目录>         覆盖签名的标准路径的位置
  -proc:{none,only}          控制是否执行注释处理和/或编译。
  -processor <class1>[,<class2>,<class3>...] 要运行的注释处理程序的名称; 绕过默认的搜索进程
  -processorpath <路径>        指定查找注释处理程序的位置
```

**java -version**

```
C:\Users\86137>java -version
java version "1.8.0_202"
Java(TM) SE Runtime Environment (build 1.8.0_202-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.202-b08, mixed mode)
```

注意：**每修改一次环境变量，需要打开一个新的命令行窗口测试**



## Idea  Eclipse

集成开发环境

IDEA 32位 64位都可以安装， 64位选择64位启动的快捷方式（Launcher）



## JVM

 Java virtual machine java

虚拟机，保证跨平台,平台无关性,一次编译，多个平台都可以运行。

一次编译，多处运行

![](E:\习题\Java\img\day01-01.png)

.Java文件  -> 编译生成.class文件（字节码文件，二进制文件）-》加载到 JVM 中运行

JVM  

- Windows  
- Mac
- Linux

每个操作系统中安装JVM后，都可以加载class文件执行，Java具有跨平台特性，一次编译，多处运行

## [JDK、JRE、JVM三者间的关系](https://www.cnblogs.com/zhangzongxing01/p/5559126.html)

## GC

Garbage collection垃圾回收机制, C,C++,free delete需要手动调用，释放存储空间/对象/连接池---内存资源

Java里面垃圾（资源）自动回收，垃圾资源即时置空=null

```
Student stu = new Student();    创建了一个对象，需要分配空间
stu = null;     回收的前提，这样之后会根据JVM内部的垃圾回收算法，当达到一定条件开始回收这些资源
```

## IDEA 开发

src ：源代码目录，今后写代码都在src中写

src-》右击-》new-》Java class

**psvm** -> 生成main方法

java应用程序的入口，如果要执行这个java文件，一定是从这个main方法开始执行

```
public static void main(String[] args) {
        
}
```

**sout**

```
System.out.println();
```

```
System.out.println("Hello World");
```



## 改编码

GBK          国标码

UTF-8       万国码   国际通用编码

编码不一致会导致乱码

File -> Settings -> Project Encoding / Default Encoding for .....

## 包

package

大学

院 - 系 - 班

Java项目 -》 Java文件成千上万个-》想找某一个文件就有困难-》分包管理

后期：service dao pojo/bean  contrller ........

每个包中放跟这个包名关系密切的相关文件

按天分包 -》 包怎么命名-》 域名倒置

域名？  baidu.com   sina.com  -> com.baidu

实际工作中，项目的命名   com.baidu.ai.car.项目模块

src->右击-》new-》package-》com.train.day01





## 标识符

作用：用来起名字-》变量  方法名   类名 ......

规则：由字母、数字、下划线，美元符$组成，且数字不能作为开头

abc -》 是  1abc-》不是     

java中命名规则：

- 类名：由有意义的单词组成，并且每个单词的首字母都要大写，如：HelloWorld    OrderInfo（大驼峰）
- 变量名：由有意义的单词组成，第一个单词的首字母小写，其后遇到的每个单词的首字母大写，如orderList，userOrderInfo
- 方法名：同变量名

**注意** 优雅的代码  UGLY的代码 ，运算符号，如果 + - =，两边加上空格

String.java 覆盖JDK为我们提供的String类

​	String str = "aa";

## 变量

写代码，一定会涉及到数据的计算，存储，保存在变量里。

java是一种强类型的语言：什么类型的变量，装载什么类型的数据

语句的结束，使用分号 ;

### 局部变量

定义方法内的变量，方法后的```{```开始，到```}```结束，都是属于这个方法的范围

作用域：他所处的方法内，也就是最近的```{ ```到``} ``结束

注意：**局部变量在使用前必须先初始化，初始化就是给它赋值**

### 全局变量/成员变量

定义在类的```{ }```之间的

作用域：整个类中都可以访问

注意：**在包含static 的方法中，类的加载，先加载 静态成员  在加载非静态成员，带有static修饰的就是静态成员**

全局变量与局部变量重名的情况下，优先使用局部变量，**就近原则**



## 基本数据类型

8种，字母全部小写

```
byte short int long float double boolean char
 1     2     4    8    4     8      1      2
 byte short int long   整数
 float double          浮点数/小数
 boolean               true / flase
 char                  字符
 表数范围   浮点数带有幂次方位
```

### 默认值

```
public class BasicDataType {

    static int iVal;  // 0  整数都是0
    static double dVal; // 0.0   浮点数都是0.0
    static boolean bVal; // false
    static char cVal; //   空白（啥也没有）


    public static void main(String[] args) {

        // 局部变量在使用前必须先初始化，初始化就是给它赋值
        // 优雅的代码  UGLY的代码
        // ctrl + d 复制上面一行代码到下面一行
        // ctrl + y  删除一行
        System.out.println(iVal);
        System.out.println(dVal);
        System.out.println(bVal);
        System.out.println(cVal);
    }
}
```

## float与long

```
float f = 14.5f;
// 不加L的情况下，默认是int类型的，当超出int类型的表数范围时就会报错
// 故我们在声明long类型的数据时，都要给他加上L，推荐使用大写的L，小写的容易混淆
long l = 100000000000L;
```

### 类型转换

**自动转换**

```
// 100 默认是int类型，java是一种强类型的语言，类型与变量要一致
// 将100自动转换为long类型，再赋值给x
// 为什么能够自动转换？long类型的表数范围大于int类型
long x = 100;
```

**强制转换**

加对应的数据类型，可能会精度丢失

```
double money = 12000.99;
// 12000.99->12000   精度丢失
// alt + enter    表数范围大的赋值给表数范围小的
int money1 = (int) money;
```

## 引用类型

处理基本数据类型，其他都是引用类型

String：字符串











## 运算符

算术运算符：+ , -, * / ,% ,++ ,--

```
// 自定义方法，方法名op1
    // 方法只有被调用，才会执行
    public static void op1(){
        int a = 100;
        int b = 3;
        double c = 3.0;
        // 整数 除 整数 还是整数
        System.out.println(a/b);
        // 除数 与 被除数，只要有一个是小数，结果就是小数
        System.out.println(a/c);
        // % 取余  100%3=1
        System.out.println(a % b);
    }
```

```
public static void op2(){
        int a = 10;
        int b = 10;
        // ++ 在后  10  先执行，后运算（a = a + 1）
        // a++ => a = a + 1
        System.out.println(a++);
        // ++ 在前  11    先运算，后执行
        System.out.println(++b);
    }
```

赋值运算符

=、+=、*=、/=、%=;

```
a += 5 => a = a + 5
```

位运算符：

&、 |、 ~ 、^、 << 、>>、 >>> 二进制位运算

比较运算符

<、>、>=、<=、!=（不等于）、==（判断两个值是否相等）

逻辑运算符

&&、 || 、!、 ^（异或）

短路原则：通过第一个已经可以确定结果，就不用再判断第二个了（&&  ||）

```
public static void op3(){
        // (5>3) true  (6<4) false
        // true && false => false
        // && 前后两边都为true时，才为true，其他全为false
        System.out.println( (5>3) && (6<4));
        // true || false => true
        // || 前后两边只要有一个为true时，就为true
        System.out.println( (5>3) || (6<4));
        // !  非true即false 非false即true
        System.out.println( !(5>3) );
        // ^
        //  两边不同时为true
        System.out.println( (5>3) ^ (6>8));
    }
```

**三目运算符** ：(？： )

条件? true的取值 : false 的取值

```
 public static void op4(){
        int score = 90;
        //  条件 ？结果1 : 结果2
        //  条件-》true =》结果1
        // 条件-》false =》结果2
        System.out.println((score >95) ? "优秀" :"一般");
    }
```

优先级,,,尽量用小括号,,引起,这样**可读性更好**

## IF语句

```
if 条件{
    if语句块
}
```

```
if 条件{
   //true
} else {
   // false
}
```

```
if 条件1{

} else if 条件2{
     // 条件2 为真，后续条件3与else不会在执行
} else if 条件3{

}  else {

}
.....
```

## While

不确定循环次数的，适用于while循环

```
public static void whileDemo(){
        boolean isRunning = true;


            // 创建Scanner对象，用于接收键盘输入
            // 开始接收键盘输入，必须输入内容后，代码才能向后执行
            // nextInt 表示输入的是一个整数
            // 将输入的一个整数赋值给score
        Scanner scanner = new Scanner(System.in);

        while(isRunning) {
            System.out.println("请输入0-100之间的整数，输入-1退出");
            int score = scanner.nextInt();
            if(score == -1) {
                isRunning = false;
            } else if(score < 60) {
                System.out.println("不及格");
            } else if(score >= 60 && score <75) {
                System.out.println("及格");
            } else if(score >= 75 && score < 90) {
                System.out.println("良好");
            } else if(score >= 90 && score <= 100){
                System.out.println("优秀");
            } else {
                System.out.println("输入数据不在区间内");
            }
        }


    }
```

## for

确定循环次数的，适用于for循环

```
 // 0 - 100的和
    public static void main(String[] args) {
        int sum = 0; // 和的初始值
       // fori
        // i = 0=>0<100 => 执行循环sum = 0 + 0 = 0
        // i++ => i=1 => 1 < 100 => 执行循环sum = 0 + 1 = 1
        // i++ => i=2 => 2< 100 => 执行循环sum = 1 + 2 = 3
        //
        for (int i = 0; i <= 100; i++) {
            sum = sum + i;
        }
        System.out.println(sum);
    }
```

## 作业

1、求1-100所有整数的的和（多思考）

2、输出100以内的素数

3、输出9x9的乘法表

4、猜数游戏：随机生成一个数，然后让用户从键盘读入这个数，提示用户猜大了还是猜小了，直到猜中

5、编写一个Java应用程序。用户从键盘输入一个1—9999之间的数，程序将判断这个数是几位数，并判断这个数是否是回文数。回文数是指将该数含有的数字逆序排列后得到的数和原数相同，例如12121、3223都是回文数