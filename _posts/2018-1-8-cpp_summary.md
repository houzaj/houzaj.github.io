---
layout: post
title: 'C++学习笔记'
date: 2018-01-08
author: HouZAJ
cover: ''
tags: Programming
---

> 好好学习哈哈哈哈哈哈哈

<br>

<iframe type="text/html" src="http://music.163.com/outchain/player?type=2&id=33418878&auto=0&height=66" frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86"></iframe>      

<br>

### PART I - 前言
之前C++只啃了一点很基础的东西和学了些STL的用法方便打代码哈哈哈，然后期末还是没用到 \_(:з」∠)_  
参考书籍：
1. C++ Programming -- Program Design Including Data Structures    

<br>

### PART II - 笔记
### 零散点
**C++的强制类型转换**   
```cpp
static_cast<int>(7.9 + 6.7);    //14
static_cast<char>(65);    //A
```
**cin cout**  
- **变量定义**  
头文件iostream中包含cin、cout的变量定义   
```cpp
istream cin;
ostream cout;
```  
<br>
- **读取有关函数**   
get、ignore、putback、peek函数   
```cpp    
//读取一个字符存到ch中，空格、回车均可存     
cin.get(ch);  
```
```cpp
//忽略掉下面100个字符 或者 忽略掉下个'\n'之前的所有字符   
cin.ignore(100, 'A');   
```
```cpp
//把ch变量退回输入流中    
cin.putback(ch);
```
```cpp
//检测下一个字符为何值
ch = cin.peek();
```  
<br>
- **输入失败**   
类型不匹配导致输入失败时（如将小数点'.'读入int型变量中），输入流会处于Fail State（错误状态），接下来使用该输入流的所有I/O语句都将被忽略掉，使用clear()函数可使其恢复到正常状态。
```cpp
//使cin流恢复正常状态
cin.clear();
```  
<br>
- **格式化输出**   
  - **头文件**  
  需包含头文件iomanip
  - **fixed, scientific, showpoint, setprecision**  
    - `setprecision(n)` : 将输出的小数点指定为n位，一次设置，永久生效，直到下一个setprecision(n)覆盖设置  
    - `fixed` : 固定小数点形式  
    - `showpoint` : 强制显示输出数字的小数点和小数点后面的0  
    - `scientific` : 以科学计数法形式输出  

    在标准C++的环境下，需要通过setf函数或者setiosflags函数控制符两种方式来使用  
  ```cpp
    /*
        第一种方式：最常见方式
        cout << fixed << showpoint << setprecision(2);
    */
    /*
        第二种方式：使用流函数来指定fixed, scientific, showpoint
        cout.setf(ios::fixed, ios::floatfield);
        cout.setf(ios::showpoint);
        cout << setprecision(2);
    */
    /*
        第三种方式：使用setiosflags控制符
        cout << setiosflags(ios::fixed | ios::showpoint);
        cout << setprecision(2);
    */
    cout << 1.234 << endl;      //1.23
    cout << 123456.0 << endl;   //123456.00
  ```
  ```cpp
    cout << scientific;
    cout << 123456.0 << endl;   //1.234560e+05
  ```
  ```cpp
    //取消fixed
    cout.unsetf(ios::fixed);
  ```
  - **setw, fill, setfill**  
    - `setw(n)` : 设置域宽为n  
    - `fill` : 全局设置填充字符  
    - `setfill` : 当前输出语句设置填充字符  

    ```cpp
    cout.fill('*');
    cout << setw(5) << 1 << setw(5) << setfill('#') << 2 << endl;
      //****1####2
    ```
  - **setw, fill, setfill**  
    顾名思义，左右对齐。下面以left举例，right可同理。
    ```cpp
    /*
      第一种方式：最常见方式
      cout << left;
    */
    /*
      第二种方式：使用流函数
      cout.setf(ios::left, ios::adjustfield);
    */
    /*
      第三种方式：使用setiosflags控制符
      cout << setiosflags(ios::left);
    */
    ```
    ```cpp
    //取消left
    cout.unsetf(ios::left);
    ```
- **endl, flush**  
    endl会将光标移到下一行开头('\n')，并清空缓冲区(相当于执行flush)函数  
    ```cpp
    //即使缓冲区的数据没有存满也可以显示提示信息
    int num;
    cout << "Enter an intger:" << flush;
    cin >> num;
    ```  
<br>
**文件输入/输出**  
恕我直言，OJ生成随机数据常用……  
大致模板如下：    
```cpp
// 声明流变量
ifstream inData;
ofstream outData;
// 打开文件
inData.open("xxx");
outData.open("xxx");
// Do Something
{
   //把inData当作cin， outData当作cout使用，同文本重定向操作
}
// 关闭文件
inData.close();
outData.close();
```  
可把inData改为cin，outData改为cout，这样就直接变成文本重定向   
<br>
**bool数据类型**
```cpp
bool is_valid = true;  //相当于 = 1
bool is_valid = false; //相当与 = 0
```  
<br>
**assert函数**  
终止程序执行，指出发生错误的表达式，包含错误源代码的文件名等，对提高代码质量起很大作用  
需包含头文件cassert或assert.h  
```cpp
int a = 5, b = 0;
assert(b);
cout << a/b << endl;
//输出：test3: ../test3/main.cpp:6: int main(): Assertion `b' failed.
```
```cpp
int a = 5, b = -3;
assert(b > 0);
cout << __gcd(a, b) << endl;
//输出：test3: ../test3/main.cpp:6: int main(): Assertion `b > 0' failed.
```  
另外，可在预处理指令`#include <cassert>`前加入`#define NDEBUG`取消所有assert语句   
<br>
**eof函数**  
检测输入流变量是否遇到了文件结束标志  
在遇到文件结束标志时返回true，否则返回false  
```cpp
while(!cin.eof()){
  //Do Something
}
```  
<br>
**引用参数**  
引用参数接受实参的内存地址，因此在以下三种情况中十分适用：
1. 要从参数中返回多一个值，如`扩展欧几里德算法`：
2. 实参值本身需要改动，如`交换函数`：
```cpp
void swap(int& a, int& b){
  int temp = a;
  a = b;
  b = temp;
}
```
3. 传递地址可以节省拷贝大量数据所需的内存空间和时间  
<br>
**全局变量的副作用**  
若多个函数都使用到某个全局变量，一旦出现差错，就很难发现是由哪个函数引起的  
在某个部分引起全局变量错误，易误以为是由另一部分引起的。  
<br>
**函数重载**  
函数重载为多个函数使用同个名字，每个函数必须有不同的行参列表  
```cpp
//可用函数larger判断两个int, char, double, string型变量的最大值，使用时无需使用四个函数，只需larger这一个函数
int larger(int x, int y)  {return (x > y)?x:y;}
char larger(char x, char y) {return (x > y)?x:y;}
double larger(double x, double y) {return (x > y)?x:y;}
string larger(string x, string y) {return (x > y)?x:y;}
int main(){
  //Do Something
}
```  
<br>
**枚举类型**  
略  
<br>
**typedef语句**  
创建已定义数据类型别名，常用来简化数据类型名
```cpp
typedef unsigned long long ull;
typedef long long ll;
int main(){
  ull a = 0;
  ll b = 0;
}
```  
<br>
**namespace(名字空间)**  
ANSI/ISO标准C++试图用namespace来解决全局标识符名字重复的问题
```cpp
namespace temp{
  const int a = 10;
  const int b = 20;
}
using namespace temp;   //简化使用所有该namespace成员的语法
int main(){
  cout << a << endl;
  cout << b << endl;
}
```  
```cpp
namespace temp{
  const int a = 10;
  const int b = 20;
}
using temp::a;   //简化使用某个该namespace成员的语法
int main(){
  cout << a << endl;
  cout << temp::b << endl;
}
```  
<br>
**string数据类型**  
string是C++的字符串，比起C语言中用字符数组那是简单得多，具体语法如下：    
```cpp
int len, pos;
string str_sub;
string str1 = "Hello";
string str2 = "World";
// ----------------------
string str3 = str1 + ' ' + str2;  //str3 == "Hello World"
str3[6] = 'w';  //可用下标访问与修改, str3 == "Hello world"
len = str3.length();  //获取长度，也可用str3.size();
pos = str3.find("or");  //查找子串，失败返回npos
str_sub = str3.substr(6, 5);  //返回子串，str_sub == "world"
str1.swap(str2);  //交换子串， str1 == "World", str2 == "Hello"  
```  
<br>
**定义二维数组的另一种方法**  
先用typedef定义一个二位数组数据类型，然后用该类型来定义数组  
```cpp
const int row = 20;
const int col = 10;
typedef int tableType[row][col];
tableType matrix;
```
