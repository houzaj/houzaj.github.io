---
layout: post
title: '学习笔记 - 数据结构和算法'
date: 2018-01-12
author: HouZAJ
cover: ''
tags: Programming
---

> 好好学习哈哈哈哈哈哈哈

<br>

<iframe type="text/html" src="http://music.163.com/outchain/player?type=2&id=33418878&auto=0&height=66" frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86"></iframe>      

<br>

> 目录  

* CATAlOGY
{:toc}

<br>
## PART I - 前言
这是传说中挂掉一片人的科目……    
参考：  
1. Data Structures, Algorithms, and Applications in C++  

<br>
{:toc}
## PART II - 笔记
### 线性表 —— 数组描述
#### **抽象类 linearlist**  
创建一个抽象类 linearlist 方便派生接下来的类 arraylist和以后用到的其他类  
```cpp
  template <class T>
  class linearList{
  public:
      virtual ~linearList() {}    //析构函数需要为虚函数，作用是能调用引用对象中数据类型的析构函数
      virtual bool empty()  const = 0;    //线性表为空时返回true
      virtual int size()  const = 0;      //返回元素个数
      virtual T& get(int theIndex)  const = 0;    //返回索引为 theIndex 的元素
      virtual int indexOf(const T& theElement)  const = 0;    //返回元素 theElement 第一次出现的元素
      virtual void erase(int theInex) = 0;    //删除索引为 theIndex 的元素
      virtual void insert(int theIndex, const T& theElement) = 0;  //把 theElement 插入线性表中索引为 theIndex 的位置上
      virtual void output(ostream& out)  const = 0;  //输出
  };
```
<br>
#### **改变 array1D 的长度**  
改变一个一维数组的长度需要建立一个具有新长度的数组，然后把数据copy过去，最后使原数组指向新数组，并释放原数组的空间  
当数组满而需要增大数组长度时，数组长度常常需要加倍，称为 **数组倍增**  
```cpp
  template<class T>
  void changeLength1D(T*& a, int oldLength, int newLength){
      if(newLength < 0)   return;     //非法返回
      T* temp = new T[newLength];     //新数组
      int number = min(oldLength, newLength);
      copy(a, a + number, temp);
      delete [] a;
      a = temp;
  }
```
<br>
#### **arrayList 类**   
```cpp
//arrayList类定义 ---------------------------------------------------------
  template <class T>
  class arrayList : public linearList<T>{
  public:
      arrayList(int initCap = 10);
      arrayList(const arrayList<T>&);
      ~arrayList() { delete [] element; }

      //ADT方法
      bool empty()  const { return listSize == 0; }
      int size()  const { return listSize; }
      T& get(int theIndex)  const;
      int indexOf(const T& theElement)  const;
      void erase(int theInex);
      void insert(int theIndex, const T& theElement);
      void output(ostream& out)  const;

      //其他方法
      int capacity() const   { return arrayLength; }

  protected:
      bool checkIndex(int theIndex)   const;  //索引theIndex是否有效
      T* element;         //存储线性表元素的一维数组
      int arrayLength;    //一维数组的容量
      int listSize;       //线性表的元素个数
  };
```
```cpp
// arrayList的构造函数和复制构造函数 ------------------------------------------
  template <class T>
  arrayList<T>::arrayList(int initCap){
      if(initCap < 0)     return;     //防止数据非法
      arrayLength = initCap;
      element = new T[arrayLength];
      listSize = 0;
  }

  template <class T>
  arrayList<T>::arrayList(const arrayList<T>& theList){
      arrayLength = theList.arrayLength;
      listSize = theList.listSize;
      element = new T[arrayLength];
      copy(theList.element, theList.element + listSize, element);    //STL方法
  }
```
```cpp
  // arrayList的基本方法：checkIndex, get, indexOf ------------------------------------------
  template <class T>
  bool arrayList<T>::checkIndex(int theIndex) const{
      if(theIndex < 0 || theIndex >= listSize)    return false;
      return true;
  }

  template <class T>
  T& arrayList<T>::get(int theIndex) const{
      if(checkIndex(theIndex))    return element[theIndex];
  }

  template <class T>
  int arrayList<T>::indexOf(const T& theElement)  const{
      int theIndex = (int)(find(element, element + listSize, theElement) - element);  //STL方法
      return (theIndex == listSize) ? -1 : theIndex;
  }
```
```cpp
  // 删除一个元素：即向左覆盖 ------------------------------------------------------------
    template <class T>
    void arrayList<T>::erase(int theIndex){
        if(checkIndex(theIndex)){
            copy(element + theIndex + 1, element + listSize, element + theIndex);   //向左覆盖
            element[--listSize].~T();   //???

            if(listSize < arrayLength/4)    changeLength1D(element, listSize, listSize/2);
        }
    }
```
```cpp
  // 插入一个元素：即先右移再插入，插入时若数组已满则进行倍增 ------------------------
  template <class T>
  void arrayList<T>::insert(int theIndex, const T& theElement){
      if(theIndex < 0 || theIndex > listSize)     return;

      //判定是否需要倍增，需要则进行倍增
      if(listSize == arrayLength){
          changeLength1D(element, arrayLength, 2*arrayLength);
          arrayLength = arrayLength*2;
      }

      copy_backward(element + theIndex, element + listSize, element + listSize + 1);  //STL方法
      element[theIndex] = theElement;
      listSize++;
  }
```
<br>
#### **C++ 迭代器**   
为了简化迭代器的开发和基于迭代器的通用算法的分类，C++的STL定义了5种迭代器：  
1. **输入**: 提供对其指向元素的只读操作，具有 前置++, 后置++ 等操作符
2. **输出**: 提供对其指向元素的写操作，具有 前置++, 后置++ 等操作符
3. **向前**: 具有++操作符  
4. **双向**: 具有++, --操作符  
5. **随机访问**: 最一般的迭代器，可随意实现跳跃移动，也可通过指针算术运算实现移动，但sort不支持  

所有迭代器都具备操作符 `==`, `!=`, 解引用符 `*`,

#### **arrayList 的一个迭代器**  
```cpp
// iterator 类 -------------------------------------------------
  class iterator{
  public:
      //5个typedef语句实现双向迭代器
      typedef bidirectional_iterator_tag iterator_category;
      typedef T value_type;
      typedef ptrdiff_t difference_type;
      typedef T* pointer;
      typedef T& reference;

      iterator(T* thePosition = 0) { position = thePosition; }

      T& operator*() const { return * position; }
      T* operator->() const { return & * position; }

      iterator& operator++ () { ++position; return * this; }
      iterator operator++(int) () { iterator old = * his; ++position; return old; }
      iterator& operator-- () { --position; return * this; }
      iterator operator--(int) { iterator old = * this; --position; return old; }

      bool operator!= (const iterator right)  const{ return position != right.position; }
      bool operator== (const iterator right)  const{ return position == right.position; }

  protected:
      T* position;
  };
```
