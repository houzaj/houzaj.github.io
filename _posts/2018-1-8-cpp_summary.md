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
### ▲ 零散点
**C++的强制类型转换**   
```cpp
  static_cast<int>(7.9 + 6.7);    //14
  static_cast<char>(65);    //A
```  
<br>
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
**bool数据类型**
```cpp
  bool is_valid = true;  //相当于 = 1
  bool is_valid = false; //相当于 = 0
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
      return a;
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
**string数据类型**  
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
<br>
**头文件的包含和多重包含**  
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

### ▲OOP  
**类**  
- **类定义**  
如定义clockType类
```cpp
  class clockType{
  public:
      void setTime(int, int, int);
      void getTime(int&, int&, int&);
      void printTime()    const;    //const说明不能修改clockType类型的成员变量
      void incrementSeconds();
      void incrementMinutes();
      void incrementHours();
      bool equalTime(const clockType& otherClock) const;    //传引用可提高性能（因为无需拷贝）
  private:      //私有成员不能被类外部访问
      int hr;
      int min;
      int sec;
  };
```
另外注意：  
  1. 不能在变量定义时同时初始化  
  2. 类成员函数在类内一般只用函数原型定义，因为如果在类中提供函数原型，会导致类定义变长，以至于难以理解，同时与信息隐藏有关    
<br>  

- **成员函数实现**  
以setTime函数为栗子
```cpp
  void clockType::setTime(int hours, int minutes, int seconds){
      hr = (0 <= hours && hours < 24)?hours:0;
      min = (0 <= minutes && minutes < 60)?minutes:0;
      seconds = (0 <= seconds && seconds < 60)?seconds:0;
  }
```
<br>
- **类公有成员(public) 和 私有成员(private)， 受保护成员(protected)**    
类成员分为：`公有成员(public)`，` 私有成员(private)`， `受保护成员(protected)` (protected成员在后面！)   
类中默认成员声明为私有成员，故上述类定义可写为：  
```cpp
  class clockType{
      int hr;
      int min;
      int sec;
  public:
      void setTime(int, int, int);
      void getTime(int&, int&, int&);
      void printTime()    const;
      void incrementSeconds();
      void incrementMinutes();
      void incrementHours();
      bool equalTime(const clockType& otherClock) const;
  };
```
<br>

- **构造函数(Constructor)**  
通过构造函数来保证类中数据成员的初始化，可有多个进行重载  
当声明对象时，构造函数将自动执行  
```cpp
  class clockType{
  public:
      //省略成员函数
      clockType(int, int, int);  //带参数的构造函数
      clockType();  //默认构造函数
  private:
      int hr;
      int min;
      int sec;
  };

  //相关实现
  clockType::clockType(int hours, int minutes, int seconds){
      hr = (0 <= hours && hours < 24)?hours:0;
      min = (0 <= minutes && minutes < 60)?minutes:0;
      seconds = (0 <= seconds && seconds < 60)?seconds:0;
  }
  clockType::clockType(){
      hr = 0;
      min = 0;
      sec = 0;
  }
```    
构造函数可带默认参数，也称为默认构造函数  
```cpp
  class clockType{
  public:
      //省略成员函数
      clockType(int = 0, int = 0, int = 0);  //默认构造函数
  private:
      int hr;
      int min;
      int sec;
  };

  //相关实现
  clockType::clockType(int hours, int minutes, int seconds){
      hr = (0 <= hours && hours < 24)?hours:0;
      min = (0 <= minutes && minutes < 60)?minutes:0;
      seconds = (0 <= seconds && seconds < 60)?seconds:0;
  }
```  
```cpp
  int main(){
    clockType myclock1;
    clockType myclock2(5, 12, 40);
  }
```
<br>
- **拷贝构造函数(Copy Constructor)**  
用已经存在的类对象进行初始化需使用拷贝构造函数  
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

  myVector::~myVector() { delete []p; }

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
- **析构函数(Destructor)**  
每个类只能有一个析构函数，在程序退出类对象的作用域（即类对象被释放）时，自动执行类的析构函数。
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
    delete []p;
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
- **抽象数据类型(Abstract data type, ADT)**  
只确定逻辑特性而没有实现细节的数据类型，有3个相关属性：  
  1. `类型名称(Data Type Name)`
  2. `域(Domain)`： 即属于ADT的一系列值
  3. `一系列操作(Operations) `

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
<br>

- **类与结构体**  
如果类的所有数据成员都是公有成员，不包含任何成员函数，那么一般使用结构体  
<br>

**继承("is-a"关系) 与 组成("has-a"关系)**  
- **继承**  
从现有类的基础上创建新类
  - `派生类(Derived class)` : 创建的新类，创建的新类的新类……
  - `现有类(Base class)` : 从现有类的基础上创建新类
  - `单继承(Single inheritance)` : 派生类从一个基类派生
  - `多继承(Multiple inheritance)` : 派生类从多个基类派生  

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
- **基类成员函数重定义**  
在baseClass中包含print函数，在derivedClass中也包含print函数且参数列表相同，则为基类成员函数重定义  
```cpp
  class baseClass{
  public:
      void print()    const;
      baseClass() {base_var = "base_var";}
  private:
      string base_var;
  };

  class derivedClass: public baseClass{
  public:
      void print()    const;
      derivedClass()  {derived_var = "derived_var";}
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
- **基类和派生类的构造函数**  
派生类执行自身构造函数和触发基类构造函数代码如下，值得一提的是，派生类会先调用基类构造函数，再调用自身构造函数
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
  baseClass::baseClass(string var = "base_bar by default"){    //基类构造函数
      base_var = var;
  }

  derivedClass::derivedClass(string var = "derived_var",
   string var2 = "base_var provoked by derivedClass"):baseClass(var2) {   //触发基类构造函数
      derived_var = var;
  }

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
- **类保护成员(protected)**    
可访问性介于public和private之间，派生类可以直接访问基类protected成员   
<br>
- **继承：public, private, protected**  
假设B Class 从 A Class派生，A成员在B成员中的属性如下表    
下表中√表示可以直接访问，〇表示可以间接访问，×表示不能访问（除友元函数）   
（友元函数通吃一切！）

|  memberAccessSpecifier A  |  A's public      |  A's protected  |  A's private  |
| :-----------------------: | :--------------: | :-------------: | :-----------: |
|          public           |  √ (public)      |  〇 (protected) |      ×        |
|          protected        |  〇 (protected)  |  〇 (protected) |      ×        |
|          private          |  〇 (protected)  |  〇 (protected) |      ×        |

<br>
- **组成**  
如在personType类中包含dataType类和personalType类，代码如下  
```cpp
  //datatype类-------------------------------------------------------
  class dateType{
  public:
      dateType(int year, int month, int day);
      void print();
  private:
      int dYear;
      int dMonth;
      int dDay;
  };

  dateType::dateType(int year = 2018, int month = 2, int day = 30){   //dataType的默认构造函数
      dYear = year;
      dMonth = month;
      dDay = day;
  }

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

  personalType::personalType(string name = "null"){
      dName = name;
  }

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
- **OOD(面向对象程序设计) 和 OOP(面向对象编程)**  
  **OOD的三个基本特征**  
    1. `封装`： 把数据和数据上的操作组合在一个独立单元中的能力 (类 class)  
    2. `继承`： 在现有的对象基础上创建新的对象的能力 (继承inheritance, 组合)  
    3. `多态`： 使用相同表达式指定不同操作的能力  

<br>
