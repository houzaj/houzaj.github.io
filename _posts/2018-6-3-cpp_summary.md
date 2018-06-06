---
layout: post
title: '学习笔记 - C++'
date: 2018-6-3
author: HouZAJ
cover: 'http://houzajblog-1252277898.coscd.myqcloud.com/20180108%20CPP/%E6%9C%AA%E6%A0%87%E9%A2%98-2-01.png'
tags: Programming
---

> C++ 是个无底洞……  

<br>

<iframe type="text/html" src="http://music.163.com/outchain/player?type=2&id=33418878&auto=0&height=66" frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86"></iframe>      

<br>

> 目录  

* CATAlOGY
{:toc}

<br>

## 更新日志(0311 -> 0603)  
1. 修改之前的不优美写法  
2. 修正了一堆bug
3. 新增了一堆bug

## 更新日志(0108 -> 0311)  
**概念性错误**   
1. 重载 >> << 可作为类内成员  
2. 为保持与内置版本一致，后置++需返回值，前置++需返回引用  
3. 构造函数中的“也可写作”，改为“一般不写作”  
4.  **[重要]** extgcd()部分代码出错  

**具体新增内容**  
2. 重载函数调用运算符（函数对象）  
3. 转换为bool  
4. 面向对象的基本概念  
5. auto类型 和 decltype类型： 推断类型  
6. 浅拷贝与深拷贝  
7. 委托构造函数  
9. 有虚基类时的构造函数  

**新增与修改**
1. 参考加多两个  
2. 增强部分解释与联系  
3. 修改类构造函数默认书写方式  
4. STL的应用  
<br>

## PART I - 前言
之前C++只啃了一点很基础的东西和学了些STL的用法方便打代码哈哈哈，然后期末还是没用到 \_(:з」∠)_  
参考：
1. C++ Programming -- Program Design Including Data Structures    
2. Data Structures, Algorithms, and Applications in C++  
3. Object Oriented Programming with C++ (Fourth Edition)  
4. C++ Primer  
5. 学堂在线 —— C++语言程序设计进阶  

<br>

## PART II - 笔记  
### 零散点
#### **C++的强制类型转换**   
```cpp
  double x = (double) 1;  //C语言的强制类型转换
  double y = double(1);   //C++的强制类型转换
  int a = static_cast<int>(7.9 + 6.7);    //C++的强制类型转换
```
<br>

#### **新的操作符**  
对这四个我是一脸蒙蔽啊……  
```cpp
    //static_cast操作符，用于强制类型转换
    int a = static_cast<int>(7.9 + 6.7);    
```
```cpp
    //const_cast 操作符，显式改变变量的const或volatile属性
    const int constant = 1;
    int& var = const_cast<int&>(constant);
    var = 2;
    cout << "constant's value: " << constant << endl;
    cout << "var's value: " << var << endl;
    //Output:
    //constant's value: 1
    //var's value: 2
    //常量值依然不变，但两者地址相同，神奇！

    cout << "constant's address: " << &constant << endl;
    cout << "var's address: " << &var << endl;
    //Output:
    //constant's address: 0x7ffdba948dac
    //var's address: 0x7ffdba948dac
```
```cpp
    //reinterpret_cast 操作符， 无视类型强型转换，过于强大一般不建议使用
    reinterpret_cast<type> (object);
```
```cpp
    //dynamic_cast 操作符，主要应用在执行多态对象的类型转换  
    dynamic_cast<type> (object);
```
```cpp
    //typeid操作符，可用于在程序运行期间获取未知对象的类型，比如类名
    char* objectType = typeid(object).name();
```
<br>

#### **bool数据类型**
```cpp
  bool is_valid = true;  //相当于 = 1
  bool is_valid = false; //相当于 = 0
```  
<br>

#### **wchar_t数据类型**  
标准C++定义了字符类型 wchar_t，以存储16位宽的字符   
标准C++引入了一种新的字符串，称为宽字符字符串   
```cpp
  int main(){
      setlocale(LC_ALL, "");
      const wchar_t* str = L"哈哈哈哈哈哈哈嗝emmm";
      wprintf(str);
      cout << endl;
      //Output: 哈哈哈哈哈哈哈嗝emmm
  }
```
<br>

#### **auto类型 和 decltype类型： 推断类型**  
**auto**：编译器会通过初始值自动推断变量的类型，可用来缩短类型名称   
**decltype**： 定义一个变量与某一表达式的类型相同，并不用该表达式初始化变量
```cpp
  //auto的栗子
  vector<int> vec;
  auto it = vec.begin();  //auto自动推断为 vector<int>::iterator 类型
  vector<int>::iterator it2 = vec.begin();    //等价于上一句
```
```cpp
  // auto与decltype的比较
  // 对const
  double a_var = 3.14;
  const double a_con = 3.14;
  decltype(a_var) b_dec_var = 3.14;   // b_dec_var = 3.14
  decltype(a_con) b_dec_con = 3.14;   // b_dec_con = 3.14
  auto b_auto_var = a_var;            // b_auto_var = 3.14
  auto b_auto_con = a_con;            // b_auto_con = 3.14
  b_dec_var = 3.14;                   // ok
  //b_dec_con = 3.14;                 // error，decltype保留const
  b_auto_var = 3.14;                  // ok
  b_auto_con = 3.14;                  // ok，auto不保留const

  // 对解引用操作
  double c = 3.14;
  double* ptr = &c;
  auto d_auto = *ptr;             // d_auto是double型的，auto还原为原来的
  decltype(* ptr) d_dec = c;      // d_dec是double&型的，decltype还原为引用的
```
<br>

#### **assert函数**  
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

#### **eof函数**  
检测输入流变量是否遇到了文件结束标志  
在遇到文件结束标志时返回true，否则返回false  
```cpp
  while(!cin.eof()){    //写成 while(cin)也是可以的
    //Do Something
  }
```  
<br>

#### **内联函数**  
当其被调用时，代码将逐行展开，类似于宏展开  
```cpp
  inline double cube(double a){   //关键字inline
    return (a*a*a);
  }
```
<br>

#### **全局变量的副作用**  
若多个函数都使用到某个全局变量，一旦出现差错，就很难发现是由哪个函数引起的  
在某个部分引起全局变量错误，易误以为是由另一部分引起的。  
<br>

#### **函数重载**  
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

#### **枚举类型**  
略  
<br>

#### **typedef语句**  
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

#### **namespace(名字空间)**  
ANSI/ISO标准C++试图用namespace来解决全局标识符名字重复的问题
```cpp
  namespace temp{
    const int a = 10;
    const int b = 20;
  }
  using namespace temp;   //简化使用所有该namespace成员的语法

  int main(){
    cout << a << endl;  //简化使用
    cout << b << endl;  //简化使用
  }
```  
```cpp
  namespace temp{
    const int a = 10;
    const int b = 20;
  }
  using temp::a;   //简化使用某个该namespace成员的语法

  int main(){
    cout << a << endl;  //简化使用
    cout << temp::b << endl;
  }
```  
<br>

#### **string数据类型**  
string是C++的字符串，比起C语言中用字符数组那是简单得多，具体语法如下：    
```cpp
  int len, pos;
  string str_sub;
  string str1 = "Hello";
  string str2 = "World";

  string str3 = str1 + ' ' + str2;  //str3 == "Hello World"
  str3[6] = 'w';  //可用下标访问与修改, str3 == "Hello world"
  len = str3.length();  //获取长度，也可用str3.size();
  pos = str3.find("or");  //查找子串，失败返回npos
  str_sub = str3.substr(6, 5);  //返回子串，6为开始位置，5为长度，str_sub == "world"
  str1.swap(str2);  //交换子串， str1 == "World", str2 == "Hello"  

  str1 < str2;    //可以直接用 > >= == <= < 比较两个字符串  

  getline(cin, str1, ',');    //读取整行字符串直到遇到','，第三个参数可省略
```  
<br>

#### **定义二维数组的另一种方法**  
先用typedef定义一个二位数组数据类型，然后用该类型来定义数组  
```cpp
  const int row = 20;
  const int col = 10;
  typedef int tableType[row][col];
  tableType matrix;
```  
<br>

#### **头文件的包含和多重包含**  
```cpp
  #include <iostream>   //系统提供的头文件用 < >
  #include "myHeaderFile.h"   //用户定义的用 " "
```
```cpp
  //写头文件时使用以下格式可避免多重包含变量导致编译错误
  //Header file
  #ifndef H_test    //if not define，第二次包含时已经define就会跳过下面的代码
  #define H_test
    //Do Something
  #endif
```
<br>

#### **函数指针**  
在C++中广泛被用于动态绑定和基于事件的应用  
```cpp
  typedef void(* FunPtr)(int, int);   //typedef简化代码
  void Add(int i, int j) { cout << i << " + " << j << " = " << i + j << endl; }
  void Sub(int i, int j) { cout << i << " - " << j << " = " << i - j << endl; }
  int main(){
      FunPtr ptr;   //声明
      ptr = &Add;   //绑定Add函数
      ptr(3, 5);
      //Output: 3 + 5 = 8

      ptr = &Sub;
      ptr(5, 3);    //绑定Sub函数
      //Output: 5 - 3 = 2
  }
```
<br>

### **引用**  
#### **引用参数**  
引用参数接受实参的内存地址，因此在以下三种情况中十分适用：  
1. 要从参数中返回多一个值，如`扩展欧几里德算法`：  
```cpp
  int extgcd(int a, int b, int& x, int& y){
      int d = a;
      if(b != 0){
        d = extgcd(b, a%b, y, x);
        y -= (a / b) * x;
      }else{
        x = 1;
        y = 0;
      }
      return d;
  }
```
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

#### **引用返回**  
到C++写类时非常常用，这里在只举一个简单的栗子  
```cpp
int& max(int& a, int& b)  { return (a > b) ? a : b; }   //引用传参且返回引用
int main(){
    int a = 2, b = 1;
    max(a, b) = -1;  //将-1赋给a,b中的最大值, a = -1
}
```
另外特别注意！上栗中a, b需要引用传参，因为如果不用引用的话，函数内a,b只是拷贝出来的变量，退出作用域后销毁，返回会失效  
<br>

### **cin cout**  
#### **变量定义**  
头文件iostream中包含cin、cout的变量定义   
```cpp
  istream cin;
  ostream cout;
```  
<br>

#### **输入输出有关的函数**   
cin.get、cout.put函数  
```cpp
  //读取一个字符存到ch中，空格、回车均可存     
  cin.get(ch);  

  //输出字符
  cout.put(ch);    
```

cin.getline、cout.write函数  
```cpp
  char str[20];
  cin.getline(str, 20);  //读取到 \n 或 20-1个字符
  cout << str << endl;
  //Input:  123456 78910
  //Output: 123456 78910

  //Input:  123456 789101112131415
  //Output: 123456 789101112131
```
```cpp
  const char* str = "Apple";
  for(int i = 1; i <= 5; i++){
      cout.write(str, i);
      cout << endl;
  }
  //Output:
  //A
  //Ap
  //App
  //Appl
  //Apple
```

cin.ignore、cin.putback、cin.peek函数   
```cpp
  //忽略掉下面100个字符 或者 忽略掉下个'\n'之前的所有字符   
  cin.ignore(100, '\n');   
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

#### **输入失败**   
类型不匹配导致输入失败时（如将小数点'.'读入int型变量中），输入流会处于Fail State（错误状态），接下来使用该输入流的所有I/O语句都将被忽略掉，使用clear()函数可使其恢复到正常状态。
```cpp
  //使cin流恢复正常状态
  cin.clear();
```  
<br>

#### **格式化输出**   
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

#### **自定义的操纵器**  
```cpp
  ostream& unit(ostream& output){     //语法
      output << " GB";
      return output;
  }

  int main(){
      cout << 1 + 2 << unit << endl;
      //output: 3 GB
  }
```

<br>

### **文件输入/输出**  
##### **一般文件的输入/输出**  
恕我直言，OJ生成随机数据常用……  
大致模板如下：    
```cpp
  // 声明流变量
  ifstream inData;
  ofstream outData;

  // 打开文件
  inData.open("FilePath1");    //打开文件路径为FilePath1的文件
  outData.open("FilePath2");

  {
     // Do Something
     //把inData当作cin， outData当作cout使用，同文本重定向操作
  }

  // 关闭文件
  inData.close();
  outData.close();
```
可把inData改为cin，outData改为cout，这样就直接变成文本重定向   
<br>

#### **open的更多细节**  
open第二个参数是mode，可指定打开文件目的，具体如下：  

|   参数       |        意义    |      参数       |      意义           |
| ----------- | -------------- | -------------- | ------------------ |
| ios::app    | 添加到文件尾     | ios::nocreate  | 若不存在，打开失败    |
| ios::ate    | 打开时就到文件尾  | ios::noreplace | 若已存在，打开失败    |
| ios::binary | 二进制文件       | ios::out       | 只写                |
| ios::in     | 只读            | ios::trunc     | 若已存在，删除所有内容 |  

<br>

#### **文件指针**  
```cpp
    ifstream in;
    ofstream out;
    in.open("FilePath", ios::in);
    out.open("FilePath", ios::app);

    int pin = in.tellg();     //取得输入指针
    int pout = out.tellp();   //取得输出指针

    in.seekg(0);   //移动到指定位置，0代表文件头
    out.seekp(0);
```
seekg和seekp具体用法如下表：  
其中cur代表当前，begin为文件头，end为文件尾  

|      seek函数调用              |         行为         |
| ----------------------------- | ------------------- |
|    fout.seekg(m, ios::cur)    |    指针前移m个字节    |
|    fout.seekg(-m, ios::cur)   |    指针后移m个字节    |
|    fout.seekg(m, ios::beg)    |    移到到m+1个字节处  |
|    fout.seekg(-m, ios::end)   |    从文件尾后移m个字节 |

<br>

#### **串行的输入输出操作**  
cin.put, cout.get是和之前一样的，略  
cin.read, cout.write用于二进制形式输入输出，存储速度快  
```cpp
    ofstream out;
    ifstream in;
    out.open("/home/a/1.out");
    double arr[4] = {3.14, 6.28, 2.7100, 9.99990};
    double arr2[4] = {0};

    out.write((char*) &arr, sizeof(arr));   //语法
    out.close();

    in.open("/home/a/1.out");
    in.read((char*) &arr2, sizeof(arr2));     //同样是语法
    copy(arr2, arr2 + 4, ostream_iterator<double>(cout, " "));
    cout << endl;
    //Output: 3.14 6.28 2.71 9.9999
```
<br>

#### **文件操纵时的错误处理**  
错误处理函数如下：  

|  函数        |        返回值和意义                        |
| ----------- | ---------------------------------------- |
|  eof()      | 读取到文件尾返回true                        |
|  fail()     | 输入输出操作失败返回true                     |
|  bad()      | 有非法操作时，或有不可恢复的错误发生时，返回true |
|  good()     | 无错误返回true，返回false不能进行任何操作      |

<br>

### **[OOP] 面向对象的基本概念**  
#### **面向对象的基本概念**   
1. **对象**： 程序中用来描述客观事物的实体  
2. **抽象**： 抽象出同一类对象的共同属性和行为形成类  
3. **封装**： 把数据和数据上的操作组合在一个独立单元中的能力 (类 class)， 隐蔽对象的内部细节，只保留有限的对外接口  
4. **继承**： 在现有的对象基础上创建新的对象的能力 (继承inheritance, 组合)，意义在于软件复用  
5. **多态**： 使用相同表达式指定不同操作的能力  

<br>

#### **抽象数据类型(Abstract data type, ADT)**  
只确定逻辑特性而没有实现细节的数据类型，有3个相关属性：  
  1. **类型名称(Data Type Name)**  
  2. **域(Domain)**： 即属于ADT的一系列值  
  3. **一系列操作(Operations)**  

  由此可定义clockType抽象数据类型如下所示，从中可见，类是实现ADT的一种便利的方法
```cpp
  dataTypeName
    clockType
  domain
    each clockType value is a time of day in the form of hours, minutes, and seconds.
  operations
    Set the time.
    Return the time.
    ......
```
接下来要说的类，是实现ADT的一种便利的方法  
<br>

### **[OOP] 类**  
#### **类成员 与 类定义**  
类成员分为：  
**公有成员(public)**： 作为外部接口，任何外部函数都可访问类的public成员  
**私有成员(private)**： 只允许本类中的函数访问，类外函数不能访问（友元除外，友元函数在后面！）  
**受保护成员(protected)**：  允许派生类内函数的访问 (protected成员在后面！)   
如定义clockType类，并在其中说明为什么类是实现ADT的一种便利的方式  
```cpp
  class clockType{
  private:      //私有成员不能被类外部访问，类内部默认为private，故若private出现在public和protected前面时可以省略
      //ADT的domain
      int hr;
      int min;
      int sec;

  public:
      // ADT的Operations
      void setTime(int, int, int);
      void getTime(int&, int&, int&);
      void printTime()    const;
      void incrementSeconds();
      void incrementMinutes();
      void incrementHours();
      bool equalTime(const clockType& otherClock) const;    //传引用可提高性能（因为无需拷贝）
  };
```
另外注意：  
  1. 不能在变量定义时同时初始化  
  2. 类成员函数在类内一般只用函数原型定义，因为如果在类中提供函数原型，会导致类定义变长，以至于难以理解，同时与信息隐藏有关
  3. 当函数定义于类内时，会变为inline函数，所有内联函数的限制和局限也适用于此，故只有小函数才定义在类的定义内  

<br>

#### **成员函数实现**  
以setTime函数为栗子
```cpp
  class clockType{
    //......
  };

  void clockType::setTime(int hours, int minutes, int seconds){
      hr = (0 <= hours && hours < 24)?hours:0;
      min = (0 <= minutes && minutes < 60)?minutes:0;
      seconds = (0 <= seconds && seconds < 60)?seconds:0;
  }
```
<br>

#### **静态数据成员**  
特点：  
  1. 类的第一个对象被创建时，一次性被初始化为0  
  2. 所有实例共享一个静态成员变量  
  3. 只在类内可见  
  4. 生存期为程序整个执行期  
  5. 每个静态成员变量的类型和作用域，都定义于类的定义之外，与类关联但和对象没有关系  
  6. 声明于类内，定义于源文件内  

```cpp
    class test{
    public:
        int get()  { return a; }
        void fun() { a++; }
    private:
        static int a;
    };

    int test::a;  //静态成员函数的定义

    int main(){
        test obj1;
        test obj2;
        cout << obj1.get() << endl;
        //Output: 0

        obj1.fun();
        cout << obj1.get() << " " << obj2.get() << endl;
        //Output: 1 1
    }
```
<br>

#### **静态成员函数**  
特点：  
  1. 只能访问类内声明的其他静态成员（函数或变量）  
  2. 调用时使用类名，而非对象名  

```cpp
    class test{
    public:
        static int showA() { return a; }
        void fun() { a++; }
    private:
        static int a;
    };

    int test::a;  //静态成员函数的定义

    int main(){
        test obj1;
        cout << test::showA() << endl;
        //Output: 0

        obj1.fun();
        cout << test::showA() << endl;
        //Output: 1
    }
```
<br>

#### **常量成员函数**  
可将不改变任何类内数据的函数声明为常量成员函数  
```cpp
  void get_balance()  const;
```
<br>

#### **成员指针**  
```cpp
  int A::* ip = &A::m;   //A::* 为“指向类A的成员的指针”， &A::m 为 “类A的成员的地址”  
```

**mutable**: 使常量成员函数能够修改被mutable修饰的变量的值  
```cpp
  class test{
  public:
      test(int a): data(a), data2(0) {}
      void modify()    const;
  private:
      int data;
      mutable int data2;
  };

  void test::modify() const{
      data = 0;   //error: assignment of member ‘test::data’ in read-only object
      data2 = 0;  //ok
  }
```
<br>

#### **类与结构体**  
如果类的所有数据成员都是公有成员，不包含任何成员函数，那么一般使用结构体  
<br>

### **[OOP] 构造函数**  
#### **构造函数(Constructor)**  
通过构造函数来保证类中数据成员的初始化，可有多个进行重载  
当声明对象时，构造函数将自动执行  
构造函数应使用 **构造函数初始值列表** 初始化类成员，**赋值会先初始化后复制**（编译器不支持该成员的默认初始化就GG）
```cpp
  class clockType{
  public:
      //省略成员函数
      clockType(int hours, int minutes, int seconds);  //带参数的构造函数
      clockType();  //默认构造函数
  private:
      int hr, min, sec;
  };

  //相关实现
  //实际上无需写拷贝构造，因为不含指针数据成员
  clockType::clockType(int hours, int minutes, int seconds): hr(hours), min(minutes), seconds(sec){}

  clockType::clockType(): hr(0), min(0), sec(0) {}    // 构造函数初始化值列表
  //一般不写作 clockType::clockType()  { hr = 0; min = 0; sec = 0; }  ，因为赋值会先初始化后赋值
```    
构造函数可带默认参数，也称为默认构造函数  
```cpp
  class clockType{
  public:
      //省略成员函数
      clockType(int = 0, int = 0, int = 0);  //默认构造函数
  private:
      int hr, min, sec;
  };

  //相关实现
  clockType::clockType(int hours, int minutes, int seconds): hr(hours), min(minutes), seconds(sec){}

  int main(){
    clockType myclock1;
    clockType myclock2(5, 12, 40);
  }
```
<br>

#### **委托构造函数**  
用其他的构造函数来构造某个构造函数，如（注意：C++11特性）    
```cpp
  class clockType{
  public:
      //省略成员函数
      clockType(int hours, int minutes, int seconds);
      clockType();
  private:
      int hr, min, sec;
  };

  //相关实现
  //省略clockType(int hours, int minutes, int seconds)的实现

  clockType::clockType(): clockType(0, 0, 0) {}   
  //委托给clockType(int hours, int minutes, int seconds)进行构造
```
<br>

#### **拷贝构造函数(Copy Constructor)**  
用已经存在的类对象进行初始化需使用拷贝构造函数，十分重要的点！  
```cpp
  class myVector{
  public:
      myVector();
      myVector(const myVector& anotherVector);    //拷贝构造函数
      ~myVector();
      void print()    const;
  private:
      unsigned t_size;
      int* p;
  };

  myVector::myVector(){
      t_size = 10;
      p = new int[t_size]();
      for(int i = 0; i < t_size; i++){  p[i] = i;  }
  }

  myVector::myVector(const myVector &anotherVector){
      t_size = anotherVector.t_size;      //t_size在向量中会改变，故要拷贝
      p = new int[t_size];
      for(int i = 0; i < t_size; i++){    //深拷贝
          p[i] = (anotherVector.p)[i];    //注意需要打括号，优先级问题
      }
  }

  myVector::~myVector() { delete [] p; }

  void myVector::print() const{
      for(int i = 0; i < t_size; i++){
          cout << p[i] << " ";
      }
      cout << endl;
  }

  //主函数----------------------------------------------------------
  int main(){
      myVector vec;
      myVector vec2(vec);
      vec2.print();
      //Output:
      //0 1 2 3 4 5 6 7 8 9
  }
```
<br>

#### **移动构造函数**   
移动构造函数可将要返回的局部对象转移到主调函数,省去了构造和删除临时对象的过程   
在 **有可被利用的临时对象** 时可使用   
可参考 [C++类的特殊成员-默认/拷贝/移动构造函数](http://blog.csdn.net/shenwanjiang111/article/details/53576196)  
<br>

#### **析构函数(Destructor)**  
每个类只能有一个析构函数，在程序退出类对象的作用域（即类对象被释放）时，自动执行类的析构函数。  
有指针数据成员的类若有创建动态对象，都需要有析构函数  
```cpp
  //如在类中new了数组后，在实例退出作用域时需要将其删除，可编写析构函数delete掉
  class myVector{
  public:
    myVector();
    ~myVector();
  private:
    int* p;
  };

  myVector::myVector(){
    p = new int[10];
  }

  myVector::~myVector(){
    delete [] p;
    cout << "clear done!" << endl;
  }

  //主函数----------------------------------------------------------
  int main(){
    int n_case = 2;
    for(int i = 1; i <= n_case; i++){
        myVector vec;
    }
  }
  //Output
  //clear done!
  //clear done!
```
<br>

### **[OOP] 继承 和 组成**  
#### **继承("is-a"关系)**  
从现有类的基础上创建新类
  - **派生类(Derived class)** : 创建的新类，创建的新类的新类……  
  - **现有类(Base class)** : 从现有类的基础上创建新类  
  - **单继承(Single inheritance)** : 派生类从一个基类派生  
  - **多继承(Multiple inheritance)** : 派生类从多个基类派生  
  - **多级继承**： 基类派生出中间基类，中间基类再派生出派生类，即 A -> B -> C  
  - **层次继承**： 子类从基类派生，作为继承的基类，类似河流主流和分流，作为继承类的在主流，派生类在直流  
  - **混合继承**： 顾名思义！  

  ```cpp
    //例如：cirle类和rectangle类从shape类派生
    class circle: public shape{   //shape类的public成员变成cirle类的public成员
      //Do Something
    };

    class circle: private rectangle{    //shape类的public成员变成cirle类的private成员(private可省略)
      //Do Something
    };
  ```
<br>

#### **基类成员函数重定义**  
在baseClass中包含print函数，在derivedClass中也包含print函数且参数列表相同，则为基类成员函数重定义  
```cpp
  class baseClass{
  public:
      void print()    const;
      baseClass():base_var("base_var"){}
  private:
      string base_var;
  };

  class derivedClass: public baseClass{
  public:
      void print()    const;
      derivedClass():derived_var("derived_var"){}
  private:
      string derived_var;
   };

  void baseClass::print() const{
      cout << base_var << endl;
  }

  void derivedClass::print() const{
      baseClass::print();
      cout << derived_var << endl;
  }

  int main(){
      baseClass base_object;
      derivedClass derived_object;
      base_object.print();
      // Output:
      // derived_var

      derived_object.print();
      //Output:
      // base_var
      // derived_var
      return 0;
  }
```  
<br>
#### **基类和派生类的构造函数**  
派生类执行自身构造函数和触发基类构造函数代码如下，值得一提的是，派生类会先调用基类构造函数，再调用自身构造函数，调用析构函数时，先调用自身的再调用基类的  
```cpp
  class baseClass{
  public:
      void print()    const;
      baseClass(string var);
  private:
      string base_var;
  };

  class derivedClass: public baseClass{
  public:
      void print()    const;
      derivedClass(string var, string var2);  //var是给derviedClass的，var2是给baseClass的
  private:
      string derived_var;
  };

  // ---------------------------------------
  baseClass::baseClass(string var = "base_bar by default"): base_var(var){}    //基类构造函数

  derivedClass::derivedClass(string var = "derived_var",
   string var2 = "base_var provoked by derivedClass"):baseClass(var2), derived_var(var) {}   //触发基类构造函数

  void baseClass::print() const{
      cout << base_var << endl;
  }
  void derivedClass::print() const{
      baseClass::print();
      cout << derived_var << endl;
  }
  // ---------------------------------------
  int main(){
      baseClass base_object;
      derivedClass derived_object;
      base_object.print();
      // Output:
      // base_var by default

      derived_object.print();
      // Output:
      // base_var provoked by derivedClass
      // derived_var

      derivedClass derived_object2("derived_bar by user", "base_bar by user");
      derived_object2.print();
      // Output:
      // base_bar by user
      // derived_bar by user

      return 0;
  }
```
<br>
**[OJ 填空题] 继承自string的MyString**  
这道题惊艳到我了！  
好好学习天天向上！  
> **样例输入**  
>>  无  

> **样例输出**  
>>  1. abcd-efgh-abcd-  
>>  2. abcd-  
>>  3.   
>>  4. abcd-efgh-  
>>  5. efgh-  
>>  6. c  
>>  7. abcd-  
>>  8. ijAl-  
>>  9. ijAl-mnop  
>>  10. qrst-abcd-  
>>  11. abcd-qrst-abcd- uvw xyz  
>>  about  
>>  big  
>>  me  
>>  take  
>>  abcd  
>>  qrst-abcd-  

> **提示**
>> **提示 1：** 如果将程序中所有 "MyString" 用 "string" 替换，那么除
    了最后两条红色的语句编译无法通过外，其他语句都没有问题，而且输出和前
    面给的结果吻合。也就是说，MyString 类对 string 类的功能扩充只体现在最
    后两条语句上面。  
>> **提示 2:** string 类有一个成员函数 string substr(int start,int
    length); 能够求从 start 位置开始，长度为 length 的子串  
>> **提示 3:** C++中，派生类的对象可以赋值给基类对象，因为，一个派生
    类对象，也可看作是一个基类对象（大学生是学生）。反过来则不行(学生未
    必是大学生） 同样，调用需要基类对象作参数的函数时，以派生类对象作为实参，也是没有问题的  

```cpp
  #include <cstdlib>
  #include <iostream>
  #include <string>
  #include <algorithm>
  using namespace std;
  class MyString:public string
  {

      // 在此处补充你的代码 ------------------------------------------------------------------------
  public:
      MyString(): string() {}   //触发基类string类构造函数
      MyString(const string s): string(s) {}  //触发基类string类构造函数
      MyString(const char* s): string(s) {}   //触发基类string类构造函数
      //触发基类string类构造函数，这里把派生类对象s交给string类构造函数处理是没问题的，反之则不行
      MyString(const MyString& s): string(s) {}   
      MyString operator () (int loc, int len){    //因此这是唯一的拓展
          return this->substr(loc, len);          //但其实也是调用一下string类的方法substr然后完事
      }
      // -----------------------------------------------------------------------------------------

  };


  int main()
  {
      MyString s1("abcd-"),s2,s3("efgh-"),s4(s1);
      MyString SArray[4] = {"big","me","about","take"};
      cout << "1. " << s1 << s2 << s3<< s4<< endl;
      s4 = s3;
      s3 = s1 + s3;
      cout << "2. " << s1 << endl;
      cout << "3. " << s2 << endl;
      cout << "4. " << s3 << endl;
      cout << "5. " << s4 << endl;
      cout << "6. " << s1[2] << endl;
      s2 = s1;
      s1 = "ijkl-";
      s1[2] = 'A' ;
      cout << "7. " << s2 << endl;
      cout << "8. " << s1 << endl;
      s1 += "mnop";
      cout << "9. " << s1 << endl;
      s4 = "qrst-" + s2;
      cout << "10. " << s4 << endl;
      s1 = s2 + s4 + " uvw " + "xyz";
      cout << "11. " << s1 << endl;
          sort(SArray,SArray+4);
      for( int i = 0;i < 4;i ++ )
      cout << SArray[i] << endl;
      //s1的从下标0开始长度为4的子串
      cout << s1(0,4) << endl;
      //s1的从下标5开始长度为10的子串
      cout << s1(5,10) << endl;
      return 0;
  }
```
<br>

#### **类保护成员(protected)**    
可访问性介于public和private之间，派生类可以直接访问基类protected成员   
<br>

#### **继承：public, private, protected**  
假设B Class 从 A Class派生，A成员在B成员中的属性如下表    
下表中√表示可以直接访问，〇表示可以间接访问，×表示不能访问（除友元函数）   
（友元函数通吃一切！）

|  memberAccessSpecifier A  |  A's public      |  A's protected  |  A's private  |
| :-----------------------: | :--------------: | :-------------: | :-----------: |
|          public           |  √ (public)      |  〇 (protected) |      ×        |
|          protected        |  〇 (protected)  |  〇 (protected) |      ×        |
|          private          |  〇 (protected)  |  〇 (protected) |      ×        |

<br>

#### **多继承**  
```cpp
  class A{
  public:
      int a;
  };

  class B{
  public:
      int b;
  };

  class C: public A, public B{  //多继承语法
  public:
      int c;
  };

  int main(){
      C obj;
      obj.a;  //继承A，可访问
      obj.b;  //继承B，可访问
      obj.c;  //自身的，可访问
  }
```
<br>

#### **虚基类**  
当某类被设为虚基类时，不管虚基类和派生类间有多少继承路径，C++都会确保只有一份该类的成员被继承  
可避免多重路径继承引起的成员重复（比如菱形状的）  
```cpp
  class A{
    //...
  };

  class B1 : virtual public A{
    //...
  };

  class B2 : virtual public A{
    //...
  };

  class C : public B1, public B2{   //因为B1，B2继承A时使用虚基类，所以C最终只会继承一份A，不会引起成员重复  
    //...
  };
```
<br>

#### **有虚基类时的构造函数**   
建立对象时所指定的类称为 **最远派生类**  
虚基类的成员是由最远派生类的构造函数通过调用虚基类的构造函数进行初始化的  
在整个继承结构中,直接或间接继承虚基类的所有派生类,都必须在构造函数的成员初始化表中为虚基类的构造函数列出参数。如果未列出,则表示调用该虚基类的默认构造函数  
在建立对象时,只有最远派生类的构造函数调用虚基类的构造函数,其他类对虚基类构造函数的调用被忽略  
```cpp
  class A{
  public:
      string var_A;
      A() {}
      A(string var): var_A(var)   {}
  };

  class B1 : virtual public A{
  public:
      string var_B1;
      B1(string var): A(var + "B1 to A"), var_B1(var) {}
  };

  class B2 : virtual public A{
  public:
      string var_B2;
      B2(string var): A(var + "B2 to A"), var_B2(var) {}
  };

  class C : public B1, public B2{
  public:
      string var_C;
      C(string var): A(var + "C to A"), B1(var + "C to B1"), B2(var + "C to B2"), var_C(var)  {}
  };

  int main(){
      A obj_A("A's var ");
      B1 obj_B1("B1's var ");
      B2 obj_B2("B2's var ");
      C obj_C("C's var ");


      cout << obj_A.var_A << endl;
      // Output:
      // A's var


      cout << obj_B1.var_A << endl;
      cout << obj_B1.var_B1 << endl;
      // Output:
      // B1's var B1 to A
      // B1's var


      cout << obj_C.var_A << endl;    //B1,B2中A的构造函数被忽略，但必须写
      cout << obj_C.var_B1 << endl;
      cout << obj_C.var_B2 << endl;
      cout << obj_C.var_C << endl;
      // Output:
      // C's var C to A
      // C's var C to B1
      // C's var C to B2
      // C's var
  }
```
<br>

#### **抽象类**  
C++支持两种类：`抽象类`和`具体类`，抽象类中包含没有实现的成员函数（`纯虚函数`），纯虚函数用0作为初始值  
具体类是没有纯虚函数的类，只有具体类才可实例化，抽象类一般作为基类派生出具体类  
下面是一个具体类的栗子  
```cpp
  class linearList{
  public:
      //析构函数需要为虚函数，作用是能调用引用对象中数据类型的析构函数，如果不写只会调用基类的析构函数，派生类的不会被调用
      virtual ~linearList() {}
      virtual bool empty()  const = 0;
      virtual int size()  const = 0;
      virtual void erase(int theIndex) = 0;
  };
```
<br>

#### **组成("has-a"关系)**  
也称嵌套类  
如在personType类中包含dataType类和personalType类，代码如下  
```cpp
  //datatype类-------------------------------------------------------
  class dateType{
  public:
      dateType(int year, int month, int day);
      void print();
  private:
      int dYear, dMonth, dDay;
  };

  dateType::dateType(int year = 2018, int month = 2, int day = 30):
      dYear(year), dMonth(month), dDay(day){}   //dataType的默认构造函数

  void dateType::print(){
      cout << "Date: " << dYear << "-" << dMonth << "-" << dDay << endl;
  }

  // personalType类----------------------------------------------
  class personalType{
  public:
      personalType(string name);
      void print();
  private:
      string dName;
  };

  personalType::personalType(string name = "null"): dName(name){}

  void personalType::print(){
      cout << "Name: " << dName << endl;
  }

  //persoanlInfo类--------------------------------------------------
  class personalInfo{
  public:
      personalInfo();
      personalInfo(int year, int month, int day, string name);
      void print();
  private:
      personalType personalName;
      dateType bDay;
  };

  personalInfo::personalInfo(){}    //使用对象成员构造函数的默认值
  personalInfo::personalInfo(int year, int month, int day,
     string name):personalName(name), bDay(year, month, day){}  //向成员对象的构造函数传递参数

  void personalInfo::print(){
      personalName.print();
      bDay.print();
  }

  //主函数----------------------------------------------------------
  int main(){
    personalInfo student;
    student.print();
    //Output
    //Name: null
    //Date: 2018-2-30

    personalInfo student2(1999, 9, 9, "your nick name");
    student2.print();
    //Output
    //Name: your nick name
    //Date: 1999-9-9
  }
```
<br>

### **[OOP] 有关类的指针**   
#### **this指针**  
this指针为指向对象自己的指针  
```cpp
  class A{
  public:
      void print()   const;
      A get()   const;
      int x, y;
  };

  void A::print()  const  { cout << x << " " << y << " " << endl;}

  A A::get() const  { return  * this; }

  int main(){
      A instanceA;
      A instanceB;
      instanceA.x = 1, instanceA.y = 2;
      instanceB = instanceA.get();
      instanceB.print();
  }
```
```cpp
  //一个神奇的栗子
  class person{
  public:
      person& setFirstName(string var);   //此处返回的是引用
      person& setLastName(string var);
      void print();
  private:
      string first_name;
      string last_name;
  };

  person& person::setFirstName(string var){ first_name = var; return * this; }
  person& person::setLastName(string var){ last_name = var; return * this; }
  void person::print(){ cout << first_name << " " << last_name << endl; }

  int main(){
      person null;
      null.setFirstName("Name").setLastName("Error");  //因为返回引用且为自身*this，故可连续用两次.
      null.print();
      //Output
      //Name Error
  }
```
<br>

#### **派生类指针**  
```cpp
  class A{
  public:
      int public_data_a;
  private:
      int private_data_a;
  };

  class B: public A{
  public:
      int public_data_b;
  private:
      int private_data_b;
  };

  int main(){
      B objB;

      A* ptr = &objB;         //基类指针绑定派生类实例
      ptr->public_data_a;     //能访问基类的成员，不能访问派生类的成员

      B* ptr2 = &objB;        //派生类指针绑定派生类实例
      ptr2->public_data_a;    //都能访问！
      ptr2->public_data_b;
  }
```
<br>

#### **虚函数**  
参考1里面似乎写的不好，或者是本人愚笨...故用 [百度百科——虚函数](https://baike.baidu.com/item/%E8%99%9A%E5%87%BD%E6%95%B0/2912832?fr=aladdin) 和 [知乎——c++虚函数的作用是什么？](https://www.zhihu.com/question/23971699) 加以辅助  
在某基类中声明为 virtual 并在一个或多个派生类中被重新定义的成员函数，可实现多态性，通过指向派生类的基类指针或引用，访问派生类中同名覆盖成员函数  
虚函数的绑定发生在程序执行期间(动态绑定 Run-time Binding)，在编译时，编译器向系统提供必要信息，使得运行时系统能产生实际代码来调用相应函数  
另外注意：**如果基类包含了虚函数，基类的析构函数同时也要设为虚函数**  
```cpp
  class A{
  public:
      virtual void print();   //只需要在基类加上virtual，派生类不需要
  };

  class B: public A{
  public:
      void print();
  };

  void A::print(){
      cout << "It's in A" << endl;
  }

  void B::print(){
      cout << "It's in B" << endl;
  }

  void callPrint(A& p){   //注意使用引用或者指针传参
      p.print();
  }

  // -----------------------------------
  int main(){
      A instanceA;
      B instanceB;
      callPrint(instanceA);
      callPrint(instanceB);
      //Output：
      //It's in A
      //It's in B

      //若不加virtual，Output：
      //It's in A
      //It's in A
  }
```
<br>

#### **浅拷贝与深拷贝**  
**浅拷贝**： 实现对象间数据元素的一一对应复制。
**深拷贝**： 当被复制的对象数据成员是指针类型时,不是复制该指针成员本身,而是将指针所指对象进行复制   
应尽量避免浅拷贝而使用深拷贝，深拷贝在 **重载赋值运算符 =** 与 **拷贝构造函数** 中体现
<br>

### **[OOP] 重载**  
#### **类的友元函数(Friend Function)**  
友元函数指在类作用域范围之外的函数，**它是类的非成员函数**，但是能访问类的私有数据成员  
为了确保数据的完整性,及数据封装与隐藏的原则,建议尽量不使用或少使用友元。  
```cpp
  class B;    //前向引用声明，因为下面的函数cSwap需要用到
  class A{
  public:
      A() { x = 1; }
      //下面一句是友元函数，函数原型前需加上friend
      friend void cSwap(A& cA, B& cB);  //这是一个交换两个类数据的函数，需要用到友元函数，注意传的是引用
      int get() { return x; }
  private:
      int x;
  };

  class B{
  public:
      B(): x(2) {}
      friend void cSwap(A& cA, B& cB);
      int get(){ return x; }
  private:
      int x;
  };

  void cSwap(A& cA, B& cB){    //友元函数定义，前面不需要加上friend
      int temp = cA.x;
      cA.x = cB.x;
      cB.x = temp;
  }

  // ---------------------------
  int main(){
      A instanceA;
      B instanceB;
      cout << instanceA.get() << " " << instanceB.get() << endl;
      //Output: 1 2

      cSwap(instanceA, instanceB);
      cout << instanceA.get() << " " << instanceB.get() << endl;
      //Output: 2 1
  }
```
 <br>

#### **重载运算符限制**  
以下说明需在刷题中慢慢体会  
  1. 不能改变运算符的优先级和结合律
  2. 不能使用默认参数，不能改变运算符所需参数个数
  3. 不能创建新运算符
  4. 不能重载以下运算符   
      `.`  |  `.*`  |  `::` | `?:` | `sizeof`   
  5. 重载运算符 `()`, `[]`, `->`, `=` 的函数一定要声明为类的成员，因为他们的左值都是类对象  
  6. 重载`<<`, `>>`一定要作为非成员（友元函数），除非那个类就是起到输入输出流的作用  
  7. 假定OpOverClass类重载运算符op，则：  
    - 若op最左边的操作数不是OpOverClass类型，则重载运算符op的函数一定要作为非成员（友元）  
    - 若重载运算符op的函数是OpOverClass类的成员，则当opo用于OpOverClass类型的对象时，op最左边的操作数必须是OpOverClass类型   

<br>

#### **作为成员函数重载 +  （重载- / * 等同理）**  
以复数相加为栗子   
```cpp
    class ComplexNum{
    public:
        ComplexNum(): real(0), vir(0)  {}
        ComplexNum(int a, int b): real(a), vir(b)  {}

        // 下面是运算符+重载的函数原型，ComplexNum是返回类型，operator+其实是函数名，(const ComplexNum&) 则是右值  
        // 仔细想想，其实和普通的类内函数声明没有区别  
        // 另外注意返回不能是引用，原因看下面
        ComplexNum operator + (const ComplexNum&)   const;
        void display();
    private:
        int real;
        int vir;
    };

    //重载运算符语法如下面这一句
    // 重载+需要定义一个先的类对象返回，如果返回引用，res退出函数已被销毁，引用会失效
    ComplexNum ComplexNum::operator + (const ComplexNum& other_complex_num)  const{
        return ComplexNum(real + other_complex_num.real, vir + other_complex_num.vir);
    }

    void ComplexNum::display(){ cout << real << "+" << vir << "i" << endl; }

    // -------------------------
    int main(){
        ComplexNum num1(1, 2);
        ComplexNum num2(2, 4);
        ComplexNum num3 = num1 + num2;  //等价于 num1.operator+(num2)
        num3.display();
        //Output: 3+6i
    }
```
<br>
#### **作为成员函数重载关系运算符 ==**
```cpp
    class ComplexNum{
    public:
        ComplexNum(int a, int b): real(a), vir(b)  {}
        bool operator == (const ComplexNum&)   const;
    private:
        int real, vir;
    };

    bool ComplexNum::operator == (const ComplexNum& other_complex_num)  const{
        return (real == other_complex_num.real && vir == other_complex_num.vir);
    }

    // -------------------------
    int main(){
        ComplexNum num1(1, 2);
        ComplexNum num2(1, 3);
        cout << ((num1 == num2)?"TURE":"FALSE") << endl;
        //Output: FALSE
    }
```
<br>
#### **作为非成员函数重载 +**  
  把上面的复数重载+拿下来改改  
  ```cpp
    class ComplexNum{
    public:
        ComplexNum(): real(0), vir(0)  {}
        ComplexNum(int a, int b): real(a), vir(b)  {}
        friend ComplexNum operator + (const ComplexNum& first, const ComplexNum& second)   ;  //函数原型1
        friend ComplexNum operator + (const ComplexNum& first, const int& second)   ;  //函数原型2
        friend ComplexNum operator + (const int& second, const ComplexNum& first)   ;  //函数原型3
        void display();
    private:
        int real;
        int vir;
    };

    //分别重载complex + complex, complex + int, int + complex三种情况
    ComplexNum operator + (const ComplexNum& first, const ComplexNum &second){
        return ComplexNum(first.real + second.real, first.vir + second.vir);
    }

    ComplexNum operator + (const ComplexNum& first, const int& second){
        return ComplexNum(first.real + second, first.vir);
    }

    ComplexNum operator + (const int& second, const ComplexNum& first){
        return ComplexNum(first.real + second, first.vir);
    }

    void ComplexNum::display(){ cout << real << "+" << vir << "i" << endl; }

    // ---------------------------------------------
    int main(){
        ComplexNum num1(1, 2);
        ComplexNum num2(2, 4);
        ComplexNum num3 = 4 + num1 + num2 + 3;
        num3.display();
        //Output: 10+6i
    }
  ```
<br>

#### **重载流插入(<<)和流析取(>>)运算符**  
仍以复数为栗子（这个栗子太好举了！）
```cpp
  class ComplexNum{
  public:
      ComplexNum(int a, int b): real(a), vir(b)  {}
      //<< >> 重载的函数原型如下
      //注意ostream和istream是不可以改变的，其为输入输出流
      friend ostream& operator << (ostream& ostreamObject, const ComplexNum& num);
      friend istream& operator >> (istream& istreamObject, ComplexNum& num);  //注意没有const，const就不能输入了！
  private:
      int real, vir;
  };

  ostream& operator << (ostream& ostreamObject, const ComplexNum& num){
      ostreamObject << "(" << num.real << "," << num.vir << "i)";
      return ostreamObject;   //注意return，后续可能接着用到
  }

  istream& operator >> (istream& istreamObject, ComplexNum& num){
      istreamObject >> num.real >> num.vir;
      return istreamObject;
  }

  // -------------------------
  int main(){
      ComplexNum num1(1, 2);
      ComplexNum num2(2, 4);
      ComplexNum num3(0, 0);
      cin >> num3;
      //Input: 3 6

      cout << num1 << " + " << num2 << " = " << num3 << endl;
      //Output: (1,2i) + (2,4i) = (3,6i)
  }
```
<br>

#### **重载赋值运算符 =**  
重载 = 运算符，可避免有指针数据成员的类的浅拷贝  
这次以之前用过的向量类来举例子  
```cpp
  class myVector{
  public:
      myVector(int start);
      ~myVector();
      void print()    const;
      const myVector& operator = (const myVector& other);
  private:
      unsigned t_size;
      int* p;
  };

  myVector::myVector(int start){
      t_size = 5;
      p = new int[t_size]();
      for(int i = 0; i < t_size; i++){  p[i] = start + i;  }
  }

  myVector::~myVector() { delete []p; }

  void myVector::print() const{
      for(int i = 0; i < t_size; i++){
          cout << p[i] << " ";
      }
      cout << endl;
  }

  myVector& myVector::operator = (const myVector& other){
      if(this != &other){     //避免自身复制，浪费时间空间
          t_size = other.t_size;
          for(int i = 0; i < t_size; i++){    //深拷贝
              p[i] = (other.p)[i];
          }
      }
      return * this;
  }

  //主函数----------------------------------------------------------
  int main(){
      myVector vec(0);
      myVector vec2(10);
      vec.print();
      vec2.print();
      //Output:
      //0 1 2 3 4
      //10 11 12 13 14

      vec = vec2;
      vec.print();
      vec2.print();
      //Output:
      //10 11 12 13 14
      //10 11 12 13 14
  }
```
<br>

#### **重载运算符 []**  
```cpp
  //重载下标[]，以向量为栗
  class myVector{
  public:
      myVector(int start);
      ~myVector();
      void print()    const;
      int& operator[] (const int& i);     //重载下标[]
  private:
      unsigned t_size;
      int* p;
  };

  myVector::myVector(int start){
      t_size = 5;
      p = new int[t_size]();
      for(int i = 0; i < t_size; i++){  p[i] = start + i;  }
  }

  myVector::~myVector() { delete [] p; }

  void myVector::print() const{
      for(int i = 0; i < t_size; i++){
          cout << p[i] << " ";
      }
      cout << endl;
  }

  int& myVector:: operator[] (const int& i){
      return p[i];
  }

  //主函数----------------------------------------------------------
  int main(){
      myVector vec(0);
      myVector vec2(10);
      cin >> vec[0];
      vec.print();
  }
```
<br>

#### **重载运算符 ++**  
```cpp
  //重载自增运算符
  class ComplexNum{
  public:
      ComplexNum(int a, int b)   {real = a, vir = b;}

      // 重载前置自增，自减同理，注意为与内置版本一致，需返回引用
      ComplexNum& operator ++();

      // 重载后置自增，自减同理，其中int i无实际意义，仅起标识为后置的作用，注意为与内置版本一致，需返回值   
      ComplexNum operator ++(int);  
      void display();
  private:
      int real, vir;
  };

  ComplexNum& ComplexNum::operator ++(){
      real++;
      return * this;
  }

  ComplexNum ComplexNum::operator ++(int){
      ComplexNum temp = * this;
      real++;
      return temp;
  }

  void ComplexNum::display(){ cout << real << "+" << vir << "i" << endl; }

  // -------------------------
  int main(){
      ComplexNum num1(1, 2);
      ComplexNum num2(2, 4);
      num1.display();
      num2.display();
      //Output:
      //1+2i
      //2+4i

      num1++;   // 相当于 num1.operator++(0);
      ++num2;   // 相当于 num2.operator++;
      num1.display();
      num2.display();
      //Output:
      //2+2i
      //3+4i
  }
```
<br>

#### **重载成员访问运算符 \***  
略  
<br>

#### **重载函数调用运算符（函数对象）**
像使用函数一样使用该类对象   
```cpp
  struct myAbs{
      int operator () (int val){
          return (val < 0) ? -val : val;
      }
  };

  int main(){
      cout << myAbs()(-42) << endl;
      //Output: 42
  }
```
```cpp
  // 函数对象经常作为泛型算法的实参
  struct printTwice{
    char op;
    printTwice(char ch = ' '): op(ch) {}
    void operator () (const string& obj) const{
        cout << obj << op << obj << endl;
    }
  };

  int main(){
    printTwice(',')("123");
    //Output: 123,123

    string arr[3] = {"abc", "def", "hhh"};
    for_each(arr, arr + 3, printTwice());
    //Output:
    //abc abc
    //def def
    //hhh hhh
  }
```
<br>

### **[OOP] 类型转换**  
#### **基本类型 -> 类**  
```cpp
  class myString{
  public:
      myString(char* str);
      myString() {}
  private:
      char* p;
  };

  myString::myString(char* str){
      int length = strlen(str);
      p = new char[length + 1];
      strcpy(p, str);
  }

  int main(){
      myString s1 = myString((char*)("Apple"));  //从char*类型到myString类型，通过隐式调用构造函数
  }
```
#### **类 -> 基本类型**  
重载类型转换符函数  
```cpp
  //以立方转原来的数为栗子
  class cube{
  public:
      cube(double num = 0): res(num*num*num) {}
      operator double() { return pow(res, 1.0/3) ; }    //重载类型转换符
  private:
      double res;
  };

  int main(){
      cube num(double(4.0));
      cout << double(num) << endl;
      //Output: 4
  }
```
<br>
#### **类A -> 类B**  
  类A转换操作符，类B使用构造函数  
  ```cpp
  //以立方转开方为栗子
  class square{
  public:
      square(double num = 0): res(sqrt(num)) {}
      double get() { return res; }
  private:
      double res;
  };


  class cube{
  public:
      cube(double num = 0): res(num*num*num) {}
      operator square();    //重载操作符
  private:
      double res;
  };


  cube::operator square(){
      return square(pow(res, 1.0/3));
  }

  // ---------------
  int main(){
      cube num(double(4.0));
      square num2(num);
      cout << num2.get() << endl;
      //Output: 2
  }
```
<br>

#### **转换为bool**  
使用流对象时都会使用为IO类定义的 operator bool， 如语句 `while(cin >> n)` 中， 为了对条件求值，cin被istream operator bool类型转换函数隐式地执行了转换。如果cin.good() == true，则返回真  
<br>

#### **显式类型转换运算符**  
**explicit**： 阻止单参数构造函数调用时执行的隐式转换  
```cpp
  class test{
  public:
      explicit test(int a): data(a) {}
  private:
      int data;
  };

  int main(){
      test obj1(100);     //合法
      test obj2 = 200;    //error: conversion from ‘int’ to non-scalar type ‘test’ requested
  }
```
<br>

**[OJ 填空题]  你真的搞清楚为啥 while(cin >> n) 能成立了吗？**    
> **题目描述**  
>> 读入两个整数，输出两个整数 ，直到碰到-1  

```cpp
  class MyCin{
  public:
      MyCin() { flag = 1; }
      MyCin& operator >> (int& obj){    // 重载运算符 >>，作为输入流因此为类成员
          cin >> obj;
          if(obj == -1)   flag = 0;
          return (* this);
      }

      operator bool(){    // 重点！ 重载操作符bool，适用于 while(),  if() 等或者显示的 bool()调用转换
          return flag;
      }

  private:
      int flag;   //标记是否遇到-1
  };

  int main()
  {
      MyCin m;
      int n1,n2;
      while(m >> n1 >> n2)
          cout  << n1 << " " << n2 << endl;
      return 0;
  }
```
<br>

### **[OOP] 模板**  
#### **函数模板**  
C++提供函数模板简化重载函数的过程  
以之前写过的larger函数重载为栗  
```cpp
  template <class T>  //模板语法，T是类型，由传参决定
  T larger(T x, T y) { return (x > y)?x:y; }

  // -------------------------
  int main(){
      cout << larger(5, 7) << endl;
      //Output: 7

      cout << larger('A', 'Z') << endl;
      //Output: Z

      cout << larger(9.9999, 9.9898) << endl;
      //Output: 9.9999
  }
```
<br>

**[OJ 填空题]  简单的SumArray**  
> **输入**    
>> 无  

> **输出**    
>> TomJackMaryJohn  
>> 10  

```cpp
  #include <iostream>
  #include <string>
  using namespace std;
  template <class T>
  T SumArray(

  // 在此处补充你的代码 ---------------------------
   T * begin, T * end){
      T res = * begin;
      T * it = begin + 1;
      while(it != end){
          res = res + * it;   //注意string类重载了+，所以这样子是可以的
          it++;
      }
      return res;
  // -------------------------------------------
  }
  int main() {
      string array[4] = { "Tom","Jack","Mary","John"};
      cout << SumArray(array,array+4) << endl;
      int a[4] = { 1, 2, 3, 4};  //提示：1+2+3+4 = 10
      cout << SumArray(a,a+4) << endl;
      return 0;
  }
```
<br>

#### **类模板**  
```cpp
template <class T>    //语法需要
  class point{
  public:
      point();
      void setPoint(const T ix, const T iy);
      void display()  const;
  private:
      T x;
      T y;
  };

  template <class T>
  point<T>::point(): x(0), y(0) {}   //注意是point<T>，并且每一处有使用T的地方，前面都要加 template <class T>

  template <class T>
  void point<T>::setPoint(const T ix, const T iy){ x = ix; y = iy; }

  template <class T>
  void point<T>::display() const{ cout << "x is " << x << ", y is " << y << endl;}

  // -------------------------
  int main(){
      point<int> p1;
      point<double> p2;
      p1.setPoint(5, 6);
      p2.setPoint(3.1415, 2.71);

      p1.display();
      //Output: x is 5, y is 6

      p2.display();
      //Output: x is 3.1415, y is 2.71
  }
```
<br>

#### **带多个参数的类模板和函数模板**  
```cpp
  //类模板
  template <class T1, class T2>   //两个参数
  class test{
  public:
      test(T1 x, T2 y): a(x), b(y) {}
      void print() { cout << a << " and " << b << endl;}
  private:
      T1 a;
      T2 b;
  };

  int main(){
      test<string, double> obj1("HAhhhhhh", 1.234567);
      obj1.print();
      //Output: HAhhhhhh and 1.23457
  }
```
```cpp
  //函数模板
  template <class T1, class T2>   //两个参数
  void print(T1 x, T2 y){ cout << x << " or " << y << endl; }

  int main(){
      print("emmm", 12580);
      //Output: emmm or 12580
  }
```
<br>

#### **无类型模板参数**  
```cpp
  template <class T, int size>    //size作为参数传进去
  class array{
  public:
      array() { arr = new T[size]; }
  private:
      T* arr;
  };

  int main(){
      array<string, 10> obj1;
  }
```
<br>

### **异常处理**  
#### **基本概念**  
异常分为 **同步异常** 和 **异步异常**， 异步异常指程序控制力之外的事件产生的错误  
C++中提出的异常处理机制 **只用来应付同步异常**  
异常处理机制：  
1. 发现异常
2. 抛出异常  
3. 捕捉异常  
4. 处理异常  

<br>

#### **异常处理机制**  
C++异常处理机制基本建立在三个关键词上：`try`, `throw`, `catch`  
**try块**： 块内可能产生错误  
**throw语句**： 抛出异常   
**catch块**： 必须紧跟在try块后，捕捉异常  
以经典的除0错误举栗子  
```cpp
  int a, b;
  cin >> a >> b;
  try{
      if(b != 0){ cout << "a/b = " << a/b << endl;}
      else{ throw(b); }
      cout << "Try END" << endl;  //发生异常时没有被执行，因为流程从try块中退出，转到catch块中
  }
  catch(int i){
      cout << "Exception caught: b = " << b << endl;
  }
  cout << "END" << endl;  //仍然执行

  //Input: 7 1
  //Output:
  //a/b = 7
  //Try END
  //END

  //Input: 5 0
  //Output:
  //Exception caught: b = 0
  //END
```
<br>

#### **捕捉机制**  
catch块会捕捉和catch参数类型匹配的异常，捕捉成功则catch块中的代码会被执行  
若catch块中的参数有名称，则该参数可用在异常处理的代码中  
```cpp
    char ch;
    cin.get(ch);
    try{
        if(isalpha(ch))     throw 'a';
        else if(isdigit(ch))    throw 1;
        else if(isspace(ch))    throw 1.0;
    }
    catch(char){
        cout << "Catch a Alpha!" << endl;
    }
    catch(int){
        cout << "Catch a Number!" << endl;
    }
    catch(double){
        cout << "Catch a Space!" << endl;
    }
    //Input: z
    //Output: Catch a Alpha!

    //Input: 5
    //Output: Catch a Number!

    //Input: (空格，打出来你也看不见)
    //Output: Catch a Space!
```
<br>

#### **重新抛出异常**  
直接调用throw即可，无需任何参数  
```cpp
  void divide(int x, int y){
      try{
          if(y != 0){ cout << "x/y = " << x/y << endl; }
          else{ throw y; }
      }
      catch(int){
          cout << "Divide ERROR!" << endl;
          throw;  //重新抛出
      }
  }
  int main(){
      int x, y;
      cin >> x >> y;
      try{
          divide(x, y);
      }
      catch(int){     //捕捉重新抛出的错误
          cout << "Function ERROR!" << endl;
      }
      //Input:8 0
      //Output:
      //Divide ERROR!
      //Function ERROR!
  }
```
<br>

#### **指定异常**  
C++11反对，故不写  
<br>

### **标准模板库(STL)**  
#### **容器**  
- **线性容器** ： vector, list, deque  
- **关联式容器** ： set, multiset, map, multimap  
- **衍生容器** ： stack, queue, priority-queue  

![容器1](http://houzajblog-1252277898.coscd.myqcloud.com/20180108%20CPP/1.jpg)  
![容器2](http://houzajblog-1252277898.coscd.myqcloud.com/20180108%20CPP/2.jpg)   
<br>

#### **算法**  
![算法1](http://houzajblog-1252277898.coscd.myqcloud.com/20180108%20CPP/3.jpg)  
![算法2](http://houzajblog-1252277898.coscd.myqcloud.com/20180108%20CPP/4.jpg)   
<br>

#### **迭代器**  
（这部分内容直接copy数据结构那篇文写的）  
为了简化迭代器的开发和基于迭代器的通用算法的分类，C++的STL定义了5种迭代器：  
1. **输入**: 提供对其指向元素的只读操作，具有 前置++, 后置++ 等操作符
2. **输出**: 提供对其指向元素的写操作，具有 前置++, 后置++ 等操作符
3. **向前**: 具有++操作符  
4. **双向**: 具有++, --操作符  
5. **随机访问**: 最一般的迭代器，可随意实现跳跃移动，也可通过指针算术运算实现移动，但sort不支持  
<br>

#### **函数对象**  
![函数对象](http://houzajblog-1252277898.coscd.myqcloud.com/20180108%20CPP/5.jpg)  
```cpp
  //比如在sort中
  sort(arr, arr + N, greater<int>());    //降序排序
```
<br>

### **标准模板库(STL)的应用**   
#### **string**  
**[OJ题]  H04:字符串操作**  
> **描述**  
>> 给定n个字符串（从1开始编号），每个字符串中的字符位置从0开始编号，长度为1-500，现有如下若干操作：  
>>  
>> copy N X L：取出第N个字符串第X个字符开始的长度为L的字符串。  
>> add S1 S2：判断S1，S2是否为0-99999之间的整数，若是则将其转化为整数做加法，若不是，则作字符串加法，返回的值为一字符串。  
>> find S N：在第N个字符串中从左开始找寻S字符串，返回其第一次出现的位置，若没有找到，返回字符串的长度。  
>> rfind S N：在第N个字符串中从右开始找寻S字符串，返回其第一次出现的位置，若没有找到，返回字符串的长度。  
>> insert S N X：在第N个字符串的第X个字符位置中插入S字符串。  
>> reset S N：将第N个字符串变为S。  
>> print N：打印输出第N个字符串。  
>> printall：打印输出所有字符串。  
>> over：结束操作。  
>>  
>> 其中N，X，L可由find与rfind操作表达式构成，S，S1，S2可由copy与add操作表达式构成。  

> **输入**  
>> 第一行为一个整数n（n在1-20之间）  
>> 接下来n行为n个字符串，字符串不包含空格及操作命令等。  
>> 接下来若干行为一系列操作，直到over结束。  

> **输出**  
>> 根据操作提示输出对应字符串。  

> **样例输入**  
>> 3  
>> 329strjvc  
>> Opadfk48  
>> Ifjoqwoqejr  
>> insert copy 1 find 2 1 2 2 2  
>> print 2  
>> reset add copy 1 find 3 1 3 copy 2 find 2 2 2 3  
>> print 3  
>> insert a 3 2  
>> printall  
>> over  

> **样例输出**  
>> Op29adfk48  
>> 358  
>> 329strjvc  
>> Op29adfk48  
>> 35a8  

本题用string类中的操作特别方便，故我将本题作为string类的应用题放在这儿   
看完题目后，会发现本题的难点在于 S，S1，S2 和 N，X，L 不一定直接给出，可能通过调用函数获得，所以我们可以考虑 **将获取n，x，l作为一个函数，获取s，s1,s2作为另一个函数**， 调用这两个函数，而函数内又可判断是否继续调用别的函数获取所需值，这样处理就会方便很多  

```cpp
    #include <iostream>
    #include <cctype>
    #include <cstdlib>
    #include <vector>
    #include <sstream>

    using namespace std;

    class myString{
    public:
        myString(const vector<string>& pvec): vec(pvec) {
            vec.insert(vec.begin(), string("none"));
        }
        bool op();            //对外调用接口，每一行指令调用一次op()，指令为over时返回false
    private:
        vector<string> vec;

        void insert();
        void reset();
        void print();
        void printall();
        int find();
        int rfind();
        string copy();
        string add();

        int getNXL();         //获取n，x，l
        string getS();        //获取s
    };

    int myString::getNXL(){
        string str;
        cin >> str;
        if(str == "find"){
            return find();
        }else if(str == "rfind"){
            return rfind();
        }else{
            return atoi(str.c_str());   //str.c_str():返回char*型字符串
        }
    }

    string myString::getS(){
        string str;
        cin >> str;
        if(str == "copy"){
            return copy();
        }else if(str == "add"){
            return add();
        }else{
            return str;
        }
    }

    void myString::insert(){
        string s = getS();
        int n = getNXL();
        int x = getNXL();
        vec[n].insert(x, s);    //vec[n].insert(x, s)：在第x的位置插入元素s
    }

    void myString::reset(){
        string s = getS();
        int n = getNXL();
        vec[n] = s;
    }

    void myString::print(){
        int n = getNXL();
        cout << vec[n] << endl;
    }

    void myString::printall(){
        vector<string>::iterator it = vec.begin();  //迭代器，可看作指针
        it++;                                       //第一个是“none”故需++
        for(; it != vec.end(); it++){
            cout << * it << endl;
        }
    }

    int myString::find(){
        string s = getS();
        int n = getNXL();
        string str = vec[n];
        int pos = str.find(s.c_str());  //str.find(s): 正向查找子串s第一次出现的位置，失败返回npos
        if(pos != string::npos){        //string::npos: find(), rfind() 失败返回npos
            return pos;
        }else{
            return s.size();            //s.size(): 返回s的长度,也可写作s.length()
        }
    }

    int myString::rfind(){
        string s = getS();
        int n = getNXL();
        string str = vec[n];
        int pos = str.rfind(s.c_str()); //str.rfind(s):逆向查找子串s第一次出现的位置，失败返回npos
        if(pos != string::npos){
            return pos;
        }else{
            return s.size();
        }
    }

    string myString::copy(){
        int n = getNXL(), x = getNXL(), l = getNXL();
        return vec[n].substr(x, l);     //str.substr(x, l):返回str的子串，该子串为str的第x个字符开始长度为l的字符串
    }

    string myString::add(){
        stringstream ss;      //使用stringstream实现类型转换
        string s1 = getS(), s2 = getS();
        int a = atoi(s1.c_str()), b = atoi(s2.c_str());

        int flag = 1;
        for(int i = 0; i < s1.size(); i++){
            if(s1[i] < '0' || s1[i] > '9'){
                flag = 0;
                break;
            }
        }
        for(int i = 0; i < s2.size(); i++){
            if(s2[i] < '0' || s2[i] > '9'){
                flag = 0;
                break;
            }
        }

        if(0 <= a && a <= 99999 && 0 <= b && b <= 99999 && flag){
            ss << a + b;
            string str;
            ss >> str;
            return str;
        }else{
            return (s1 + s2);
        }
    }

    bool myString::op(){
        string str;
        cin >> str;
        if(str == "over")           return false;
        else if(str == "copy")      copy();
        else if(str == "add")       add();
        else if(str == "find")      find();
        else if(str == "rfind")     rfind();
        else if(str == "insert")    insert();
        else if(str == "reset")     reset();
        else if(str == "print")     print();
        else if(str == "printall")  printall();
        return true;
    }

    // ------------------------------

    int main(){
        vector<string> vec;
        int t;
        cin >> t;
        while(t--){
            string str;
            cin >> str;
            vec.push_back(str);
        }
        myString obj(vec);
        while(obj.op());
        return 0;
    }
```
<br>

#### **map**
**[OJ题]  H06:冷血格斗场**  
> **描述**  
>> 为了迎接08年的奥运会，让大家更加了解各种格斗运动，facer新开了一家冷血格斗场。格斗场实行会员制，但是新来的会员不需要交入会费，而只要同一名老会员打一场表演赛，证明自己的实力。  
>> 我们假设格斗的实力可以用一个正整数表示，成为实力值，两人的实力值可以相同。另外，每个人都有一个唯一的id，也是一个正整数。为了使得比赛更好看，每一个新队员都会选择与他实力最为接近的人比赛，即比赛双方的实力值之差的绝对值越小越好，如果有多个人的实力值与他差别相同，则他会选择id最小的那个。  
>> 不幸的是，Facer一不小心把比赛记录弄丢了，但是他还保留着会员的注册记录。现在请你帮facer恢复比赛纪录，按照时间顺序依次输出每场比赛双方的id。  

> **输入**  
>> 第一行一个数n(0 < n <=100000)，表示格斗场新来的会员数（不包括facer）。以后n行每一行两个数，按照入会的时间给出会员的id和实力值。一开始，facer就算是会员，id为1，实力值1000000000。   

> **输出**  
>> N行，每行两个数，为每场比赛双方的id，新手的id写在前面。  

> **样例输入**  
>> 3  
>> 2 3  
>> 3 1  
>> 4 2  

> **样例输出**  
>> 2 1  
>> 3 2  
>> 4 2   

**map内部由二叉树（红黑树）封装而成，数据默认按照key值升序排序，插入和查找操作时间复杂度都为O(logN)**  
本题使用map是因为根据题意，需要按实力值排序，并且需要不断进行插入和查找操作，而给的数据比较大，所以使用map作为存储结构  
另外，本题的难点为，若按实力值排序，而map只能单一key对应单一value，而可能有不同id的人拥有相同的实力值，但考虑到每次都选择id最小的人作为对手，因此每次输入数据时可检测是否存在该key，若存在则当id小于该key时替换该key即可  

```cpp
  #include <bits/stdc++.h>
  using namespace std;

  int main(){
      map<int, int> mp;
      map<int, int>::iterator it, it_before, it_after;
      mp[1000000000] = 1;

      int t;
      cin >> t;
      while(t--){
          int id, val, ans_id;
          cin >> id >> val;

          it = mp.find(val);
          if(it != mp.end()){       //若找到，则替换为id更小的
              ans_id = it->second;
              it->second = min(it->second, id);
          }else{
              mp[val] = id;
              it = mp.find(val);
              it_before = next(it, -1);   //取it前一个的迭代器
              it_after = next(it, 1);     //取it后一个的迭代器


              if(it == mp.begin()){
                  ans_id = it_after->second;
              }else if(it == mp.end()){
                  ans_id = it_before->second;
              }else if(it_after->first - it->first == it->first - it_before->first){
                  ans_id = min(it_before->second, it_after->second);
              }else{
                  ans_id = (it_after->first - it->first < it->first - it_before->first) ? it_after->second : it_before->second;
              }
          }
          cout << id << " " << ans_id << endl;

      }
      return 0;
  }
```
