---
title: Java初学习之 Java基础
date: 2017-05-19 00:00:00 #文章生成时间，一般不改，当然也可以任意修改
categories: [JAVA]
---
Java学习的课堂笔记。现在看来，这些笔记实在是流于琐碎，也没能很好地抓住学习重点（学校考试方法是闭卷手写10个程序），实在是只顾着记背语法了。大学一年级初学习时还没有转专业，也没有想到会开这个博客，回头看看当时的笔记还是觉得很奇妙。
现在把这些笔记放上来，方便在日后的学习中参考经验、纠正错误。

## 基本数据类型 
按精度从“低”到“高”排列：  
byte  short  char  int  long  float  double
（低-高自动转换，高-低需强制转换）
<!-- more -->
###	逻辑类型：boolean
###	整数类型
byte（分配给1个字节）、short（2个字节）、int（4个字节）、long（8个字节，用后缀L来表示）
###	字符类型
char（2个字节，单引号引出）
###	浮点类型
float（4个字节，必须要有后缀`f`或`F`）  
double（8个字节，可以有后缀`d`或`D`，但允许省略）除法运算变成整数问题
### 转义字符
`\n`,`\b`,`\t`,`\'`,`\"`,`\\`

##	输入输出
### 输出
格式控制：
-   `%d`-int；
-   `%c`-char；
-   `%f`-float，小数最多6位；
-   `%s`-String。

举例：
```java
System.out.printf("格式控制部分"，表达式1，表达式2，…表达式n)；
System.out.printf("%4d,%10.2f",  12,       23.78); 
```
### 字符与unicode

输出加号时，两边如果是数字会直接相加，两边都是字符时，会输出两个字符在Unicode中位置和;
```java
System.out.print('a' + 'b');// 输出195
```
##	数组
###	声明

数组的元素类型 数组名[];
```java
float boy[];
float a[][];
```

数组的元素类型 [] 数组名;
```java
char [] cat;
char [][] b;
```

###	分配元素
```java
boy = new float[4];
boy[0] = 12;
boy[1] = 23.908F;
boy[2] = 100;
boy[3] = 10.23f;
```
或
```java
float boy[] = { 21.3f,23.89f,2.0f,23f,778.98f};
```
数组如果引用，如令`a = b`,则改变`a[1]`,那`b[1]`也会变。

###	数组长度
矩阵名`.length`(列的长度 = 行数) ;矩阵名`[0].length`(行的长度)
###	练习
#### 打印数组/矩阵

```java
void PrintArray(int []array){
for(int i : array){ //循环变量i依次取数组array的每一个元素的值（改进方式）
      System.out.printf("%5d",i);
      }
}
```
或
```java
static void PrintMatrix(int [][]matrix){//要有static
	for(int tr = 0;tr<matrix.length;tr++){//没有<=
		for(int td = 0;td<matrix[0].length;td++){
			System.out.printf(“%5d”,matrix[tr][td]);
        } 
    System.out.print("\n");
	}
}
```
或
```java
static void AddMatrix(int [][]m1,int [][]m2){//要有static
	if((m1.length == m2.length)||(m1[0].length == m2[0].length)||(m1[0].length != 0) ||(m1.length != 0)){ 
	int m[][] = new int[m1.length][m1[0].length];
		for(int tr = 0;tr<m1.length;tr++){//没有<=
			for(int td = 0;td<m1[0].length;td++){
				m[tr][td] = m1[tr][td] + m2[tr][td];
            } 
        System.out.print("\n");
		}
    }
}
```
b’’/
```java
static void MultiMatrix(int [][]m1){//要有static 
	int m2[][] = new int[m1[0].length][m1.length];
		for(int tr = 0;tr<m1.length;tr++){//没有<=
			for(int td = 0;td<m1[0].length;td++){
				m2[td][tr] = m1[tr][td];
            } 
            System.out.print("\n");
		}
    }
}
```
#### 选择法排序数组/折半法/数组倒置
遍历法
```java
static void sort(int a[]) {
    for(int i=0; i<a.length; i++) {
        for(int j = i+1; j < a.length;j++){
            if(a[j] < a[i]){
                int t = a[j];
                a[j] = a[i];
                a[i] = t;
            }
        }
	}
} 
```
使用时：
```java
int m[] = {123,45,64,25,867,342,87,65235,85,8};
sort(m);PrintArray(m);//sort(m)会改变m
```
折半法
```java
int start=0;
int count =0 ;
int end = a.length-1;//涉及0/start和a.length/end需注意
int middle=(start+end)/2;
while(number!=a[middle]){
    if(number>a[middle])
        start=middle;
    else if(number<a[middle])
        end=middle;
        middle=(start+end)/2;
        count++;
    if(count>N/2)//注意是count和N/2的关系来判断是否跳出循环
        break;
    }
    if(count>N/2) //注意是count和N/2的关系来判断是否跳出循环
        System.out.printf("%d不在数组中.\n",number);
    else
        System.out.printf("%d在数组中.\n",number);
}
```
数组倒置
```java
static void adverse(int a[]){
    int start = 0;
  	int N = a.length-1;
  	int end = N-start;
  	for(int i = 0;i<N/2;i++){
			int t = a[start];
			a[start] = a[end];
			a[end] = t;
			start++;
			end = N-start;
    }
}
```
或
```java
static void sort(int []array) {
    for(int i = 0;i < array.length/2;i++){
         int temp = array[i];
         array[i] = array[array.length-1-i];
         array[array.length-1-i] = temp;
         System.out.println(array);
    }
}
```
####	替换插入截取数组
替换
```java
static char[] replace(char[] arr,char c1,char c2){
    for(int i=0;i<arr.length;i++){
        if(arr[i] == c1){
            arr[i] = c2;
        }
    }
    System.out.print(arr);
    return arr;
}
```
插入
```java
// 函数char[] insert(char[] arr, char[] arr1, int index):将数组arr1插入到数组arr的第index个位置，注意数组越界的控制。
static char[] insert(char[] arr, char[] arr1, int index) {
	char[] result = new char[arr.length + arr1.length];
	for (int i=0; i<index; i++){
		result[i] = arr[i];
	}
	for (int j=0; j < arr1.length; j++){
		result[j+index] = arr1[j];
	}
	for (int k=index; k < arr.length; k++){
		result[k+arr1.length] = arr[k];
	}//一定是3个for否则会生成超长的一段
	return result;
}
```
截取
```java
// 函数char[] substr(char[] arr,int start, int end):截取字符数组arr中的从start位置开始，到end之前（不包含end）的所有字符，注意数组越界的判断。
static char[] substr(char[] arr, int start, int end) {
	char[] result = new char[end - start];
	for (int i = 0; i < result.length; i++) {
		result[i] = arr[i + start];
	}
	return result;
}
```
##	语句

```
### for循环语句
​```java
  for (开始条件;循环条件;表达式) {
        若干语句 
    } 

```
### while、do-while循环语句
```java
while (表达式) {
    若干语句 
}
do {
    若干语句
} while(表达式); 
```
举例：（《JAVA2 实用教程（第四版）》第三章 例6）

计算1+1/2!+1/3!+1/4!  … 的前20项。
```java
double sum = 0,item = 1;
int i = 1,n = 20;
while(i<=n) { 
    sum = sum+item;
    i = i+1; 
    item = item*(1.0/i);         
}
```
### if-else语句
```java
if（表达式) {
    语句
 }
else if（表达式) {
    语句
 }
… …
else {
    语句
 } 
```

###	Switch开关语句
```java
switch(表达式)
{
   case 常量值1:
               若干个语句
               break;//注意一定要break
   case  常量值2:
               若干个语句
               break;
    ...
   case  常量值n:
              若干个语句
              break;
   default:
         若干语句
}
```