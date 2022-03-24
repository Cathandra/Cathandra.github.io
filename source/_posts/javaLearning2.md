---
title: Java初学习之 类与接口、常用实用类
date: 2017-05-19 00:00:01 #文章生成时间，一般不改，当然也可以任意修改
tags: 
    - java
categories: [Back-End]
---
Java初学习的课堂笔记。

## 类与对象

### 类声明和类体

```
class 类名 {
    类体的内容（声明变量+定义方法）
}
```

###	方法

```
方法头=类型 名称（参数） {
    方法体
    返回值return ；
}
```

<!-- more -->

###	成员变量和局部变量 

This.成员变量；局部变量没有默认值 。

###	创建对象

```
声明：类名 对象名;
 +
分配变量：new运算符 类的构造方法；
```

举例：

```JAVA
People Zhubajie；
Zhubajie = new People();
//前面所述的接收键盘输入也是一种创建对象的过程
Scanner s = new Scanner(System.in);
```

###	操作变量、调用方法

```
操作变量：对象.变量；
调用方法：对象.方法（）；
```

举例：(《JAVA2 实用教程（第四版）》第四章 例9)

模拟手机和SIM卡的组合关系。涉及的类如下：
- SIM类负责创建SIM卡SIM.java 。
- MobileTelephone类负责创建手机MobileTelephone.java ，手机可以组合一个SIM卡，并可以调用setSIM (SIM card)方法更改其中的SIM卡。

```java
class SIM {
    long number;  
    SIM(long number){
        this.number = number;
    }
    long getNumber() {
        return number; 
    }
}
class MobileTelephone { 
    SIM sim;
    void setSIM(SIM card) {
        sim = card;
    } 
    long lookNumber(){
        return sim.getNumber();
    }
}
public class Example4_9 {
   public static void main(String args[]) {
       SIM simOne = new SIM(13889776509L);
       MobileTelephone mobile = new MobileTelephone();
       mobile.setSIM(simOne);
       System.out.println("手机号码:"+mobile.lookNumber()); 
       SIM simTwo = new SIM(15967563567L);
       mobile.setSIM(simTwo);
       System.out.println("手机号码:"+mobile.lookNumber());  
    }
}
```

###	实例变量和类变量

关键字static给予修饰的称作类变量，否则称作实例变量。所有对象共享类变量，通过类名直接访问类变量。  
举例：(《JAVA2 实用教程（第四版）》第四章 例10)

```java
class Lader { 
    double 上底,高;       //实例变量
    static double 下底;     //类变量,可以通过类名调用
    void 设置上底(double a) {
        上底 = a; }
    void 设置下底(double b) {
        下底 = b; }
    double 获取上底() {
       return 上底; }
    double 获取下底() {
       return 下底; }
}
public class Example4_10 { 
   public static void main(String args[]) {
       Lader.下底=100;     //Lader的字节码被加载到内存,通过类名操作类变量
       Lader laderOne=new Lader();
       Lader laderTwo=new Lader();
       laderOne.设置上底(28);
       laderTwo.设置上底(66);
       System.out.println("laderOne的上底:"+laderOne.获取上底());
       System.out.println("laderOne的下底:"+laderOne.获取下底());
       System.out.println("laderTwo的上底:"+laderTwo.获取上底());
       System.out.println("laderTwo的下底:"+laderTwo.获取下底());
    } 
}
```

###	访问权限

#### private 

不能通过`.`调用，只能在类中的函数中使用，类外部要通过函数使用

```java
class Student {
   private int age;
   public void setAge(int age) {
      if(age>=7&&age<=28) {
          this.age=age;
      }
   }
   public int getAge() {
      return age;
   }
}
```

#### protected

不同包内可直接调用；

##### friendly
不同包内不可直接调用，同包内可直接调用；

## 子类与继承

### 定义子类：

```java
    class 子类名  extends  父类名 {
           … 
     } 
```

除了Object类每个类有且仅有一个父类，一个类可以有多个或零个子类。

###	继承

同一个包中，子类继承父类的非private的成员变量，且权限不变；  
如果不在同一个包中，子类继承父类的非private的成员变量和方法，且权限不变

###	instanceof运算符

当左面的操作元是右面的类或其子类所创建的对象时，instanceof运算的结果是true，否则是false 。

###	super关键字

通过方法重写，子类可以隐藏父类的方法。如果调用被隐藏的方法，需在方法前加上super；
除此，使用super还可以调用父类的构造方法。

###	final关键字

- final 类不能有子类；

```java
final class A {
      … …
   } 
```

final方法不允许子类重写。

- final定义常量。

###	上转型对象

不能操作子类新增成员变量，不能使用子类新增方法；可以操作继承的或重写的方法。

###	abstract关键字

abstract类不能用new运算创建对象。

## 接口与实现

### 接口声明与接口体

- 声明：interface 接口的名字 ；
- 接口体：包含声明常量和抽象方法两部分。（因为是抽象方法，所以访问权限必为public）

### 实现接口

一个类通过使用关键字implements声明自己实现一个或多个接口。

```java
class A implements Printable,Addable//A实现Printable,Addable接口
```

### 接口回调

当接口变量调用被类重写的接口方法时，就是通知相应的对象调用这个方法。
举例：

```java
interface  ShowMessage {
   void 显示商标(String s);
}
class TV implements ShowMessage {
   public void 显示商标(String s) {
      System.out.println(s);
   }
}
class PC implements ShowMessage {
   public void 显示商标(String s) { 
     System.out.println(s);
   }
}
public class Example6_3 {
   public static void main(String args[]) {
      ShowMessage sm;                  //声明接口变量
      sm=new TV();                     //接口变量中存放对象的引用
      sm.显示商标("长城牌电视机");      //接口回调。
      sm=new PC();                     //接口变量中存放对象的引用
      sm.显示商标("联想奔月5008PC机"); //接口回调
   } 
}
```

##	常用实用类

###	String

#### 声明与创建

声明：`String s;`

创建：`String(s)`，`String (char a[])`，`String(char a[],int startIndex,int count)`或：

```java
char a[] = {'J','a','v','a'};
String s = new String(a);
```

#### 相关函数
- 获得字符串长度`.length();`
- 判断是否相等`.equals(str);`
- 判断前后缀`.startsWith(str);`/`.endsWith(str);`
- 比较在unicode的大小`.compareTo(str);`/`.compareToIgnoreCase();`
- 判断是否含有某些字符`.contains(str);`
- 在第几位有哪个特定的字符`.indexOf (str);`/`.indexOf (str,i);`
- 截取`.substring(i,j);`
- 获得去掉前后空格的字符串`.trim();`

举例：截取区号

```java
String number,str,numberFlag;
System.out.println("请输入查询的的座机号码，其格式为“区号-电话号码”：");
Scanner scan = new Scanner(System.in);
str = scan.next();
int i = str.indexOf('-');
numberFlag = str.substring(0,i);
System.out.println("区号是：" + numberFlag);
```

###	数值与字符串之间的转换

#### 数值-字符串

调用Byte、Integer等类进行强制类型转换：

```java
int x = Integer.parseInt(s);
```

同理：`Byte.parseByte()`、`Short.parseShort()`、`Long.parseLong()`、`Float.parseFloat()`、`Double.parseDouble()`...

#### 字符串-数值

调用`.valueOf()`函数

```java
String str = String.valueOf(12313.9876); 
```

#### 字符数组-字符串

- `.getChars(int start,int end,char c[],int offset);`// 将字符串存放到数组中
- `.toCharArray();`//将字符串中的全部字符存放在一个字符数组中
- `.getBytes();`//将当前字符串转化为一个字节数组

###	StringTokenizer类

####	`nextToken()`函数

逐个获取字符串中的语言符号（单词），字符串分析器中的负责计数的变量的值就自动减一。

#### `hasMoreTokens()`函数

只要字符串中还有语言符号，即计数变量的值大于0，该方法就返回true，否则返回false。 

#### `countTokens()`函数

得到分析器中计数变量的值。 
应用举例：

```java
String s="you are welcome(thank you),nice to meet you";
StringTokenizer fenxi=new StringTokenizer(s,"() ,"); 
int number=fenxi.countTokens();
while(fenxi.hasMoreTokens()) {
    String str=fenxi.nextToken();
    System.out.print(str+" ");
    }
System.out.println("共有单词："+number+"个");
```

###	Random类

```java
Math.random()*100;//输出(1,100)的随机数
//
Random random=new Random();
random.nextInt(100);//输出(1,100)随机数
//
random.nextBoolean();//返回一个随机boolean值
```

## 文本输入输出流

###	File类

#### 创建file对象

创建一个File对象的构造方法有3个：  
    `File(String filename);`
    `File(String directoryPath,String filename);`(注意路径中需要转义字符，以免歧义)
    `File(File f, String filename);`
e.g.

```java
File dirFile = new File(".");//"."表示当前目录
```

#### 相关函数

##### 文件相关

- 获取文件的名字`.getName()`
- 判断文件是否是可读的`.canRead()`
- 判断文件是否可被写入`.canWrite()`
- 判断文件是否存在`.exits()`
- 获取文件的长度（单位是字节）`length()`
- 获取文件的绝对路径`getAbsolutePath()`
- 获取文件的父目录`.getParent()`
- 创建新文件`.createNewFile()`
- 删除当前文件 `.delete()`

##### 路径相关

- 用字符串形式返回目录下的全部文件`.list()`(返回String类型的数组)
- 用File对象形式返回目录下的全部文件`.listFiles()`(返回file数组)  
- 用字符串形式返回目录下的指定类型的所有文件`.list(FilenameFilter obj)`(返回String类型的数组)
- 用File对象形式返回目录下的指定类型所有文件`listFiles(FilenameFilter obj)`(返回file数组)  

##### 可执行文件相关(Runtime类)

- 声明 + 创建对象：`Runtime runtime = Runtime.getRuntime()`;
- 执行：`.exec(String absolutePath)`;

### 字符输入输出流（Reader和Writer类）

举例：

```java
File sourceFile = new File("a.txt");//读取的文件
File targetFile = new File("b.txt");//写入的文件
char c[];
try{ 
    Reader in = new FileReader(sourceFile);//定义读取操作为in
    Writer out = new FileWriter(targetFile,true);//写入，out
        while(int n = in.read(c)!=-1){//固定套路，判断c[]是否为空
		    out.write =(c,0,n);//字符数组c[]中取0开始的n个量写入targetFile
        }
    out.flush();
    out.close();
}catch(IOException e){

}
```

### 缓冲流（BufferedReader和BufferedWriter类）

- 读取文本行`readLine()`
- 把字符串s写到文件中`write(String s,int off,int len) `
- 向文件写入一个回行符`newLine(); `

举例：(《JAVA2 实用教程（第四版）》第十章 例7)
```java
import java.io.*;
import java.util.*;
public class Example10_7 {
   public static void main(String args[]) {
      File fRead = new File("english.txt");
      File fWrite = new File("englishCount.txt");
      try{  Writer out = new FileWriter(fWrite);
            BufferedWriter bufferWrite = new BufferedWriter(out);
            Reader in = new FileReader(fRead);
            BufferedReader bufferRead =new BufferedReader(in);
            String str = null;
            while((str=bufferRead.readLine())!=null) {
               StringTokenizer fenxi = new StringTokenizer(str);
               int count=fenxi.countTokens();
               str = str+" 句子中单词个数:"+count;
               bufferWrite.write(str);
               bufferWrite.newLine();
            } 
            bufferWrite.close(); 
            out.close();
            in = new FileReader(fWrite);
            bufferRead =new BufferedReader(in);
            String s=null;
            System.out.println(fWrite.getName()+"内容:");
            while((s=bufferRead.readLine())!=null) {
              System.out.println(s);
           }  
           bufferRead.close();
           in.close();
      }
      catch(IOException e) {
          System.out.println(e.toString());
      }
   }
}
```