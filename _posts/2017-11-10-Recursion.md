---
layout: post
title: '递归 (Recursion) —— PPT 与 实例'
date: 2017-11-10
author: HouZAJ
cover: 'http://houzajblog-1252277898.coscd.myqcloud.com/20171110%20PRERecusion/Recursion%20Cover-01-01.png?sign=46bFsh7lC6bFk68nEg9P7/cFGRJhPTEyNTIyNzc4OTgmaz1BS0lEVXVYME83aHpET1RSQ3Z2cWNJaHk5QzY3QjdLVGNSanEmZT0xNTEzNTk2NDIyJnQ9MTUxMTAwNDQyMiZyPTE4MDAzMTM5MSZmPS8yMDE3MTExMCUyMFBSRVJlY3VzaW9uL1JlY3Vyc2lvbiUyMENvdmVyLTAxLTAxLnBuZyZiPWhvdXphamJsb2c='
tags: Programming
---

> （2017.11.17）更新了一些实例     |     
> 其实，这是一篇试iframe的文...   

<br>

<iframe type="text/html" src="http://music.163.com/outchain/player?type=2&id=28492953&auto=0&height=66" frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86"></iframe>      
<iframe type="text/html" src="http://music.163.com/outchain/player?type=2&id=34183392&auto=0&height=66" frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86"></iframe>      


### PART I - 前言   
哈哈哈这次PRE，emmmm……严！重！超！时！    
然后呢补两个例子（以后可能还会再补）    
另外关于为什么会有两首曲子……第二首太好听了忍不住就多加了一首\_(\:з」∠)\_

<br>
### PART II - 大计演讲PPT   
<iframe class="iframe-ppt" src="https://view.officeapps.live.com/op/embed.aspx?src=http%3A%2F%2Fhouzajblog%2D1252277898%2Ecoscd%2Emyqcloud%2Ecom%3A80%2F20171110%2520PRERecusion%2FRecursion%2Epptx%3Fsign%3D7A2HAkwEFgn%2FlnWztUfyu5U3vbhhPTEyNTIyNzc4OTgmaz1BS0lEVXVYME83aHpET1RSQ3Z2cWNJaHk5QzY3QjdLVGNSanEmZT0xNTEyOTA3MjI4JnQ9MTUxMDMxNTIyOCZyPTU1MzUwNTQ4NCZmPS8yMDE3MTExMCUyMFBSRVJlY3VzaW9uL1JlY3Vyc2lvbi5wcHR4JmI9aG91emFqYmxvZw%3D%3D&wdAr=1.3333333333333333" frameborder="0">This is an embedded <a target="\_blank" href="https://office.com">Microsoft Office</a> presentation, powered by <a target="\_blank" href="https://office.com/webapps">Office Online</a>.</iframe>

PPT下载： [https://mega.nz/#F!tXADUZjI](https://mega.nz/#F!tXADUZjI!Si0jjfq0RQDEvS2PKKnO5Q)

<br>

### PART III - 实例   
### 简易计算器（SZUOJ Problem 1206）    
**▲ 题目简述**      

> **Input**    
>> 输入T，表示有T个实例      
>> 接下来每行输入一个表达式，每个表达式末尾带#表示结束   

> **Output**    
>> 输出结果，保留四位小数        

> **Sample Iutput**   
>> 2    
>> 1+2\*3-4/5#    
>> (66+(((11+22)\*2-33)/3+6)\*2)-45.6789#   

> **Sample Output**    
>> 6.2000        
>> 54.3211      

**▲ 思路**    
1. **如何处理 `左括号(` 和 `右括号)` ？**       
（这里先感谢一下俊生大佬！）受俊生大佬启发，对于括号内的部分，其实和运算整体使用的方法是一样的，因此遇到左括号\"(\"可以调用自身函数，直到遇到右括号\")\"结束自身调用（即返回结果）。另外遇到\"\#\"号也应该返回结果（给主函数）。    
![](http://houzajblog-1252277898.coscd.myqcloud.com/20171110%20PRERecusion/img%202017-11-18_1.jpg?sign=CxIGQYW9RTbZ1B59tXuFBzjsq6FhPTEyNTIyNzc4OTgmaz1BS0lEVXVYME83aHpET1RSQ3Z2cWNJaHk5QzY3QjdLVGNSanEmZT0xNTEzNTkwNDMzJnQ9MTUxMDk5ODQzMyZyPTE3NDIyNDMwNjAmZj0vMjAxNzExMTAlMjBQUkVSZWN1c2lvbi9pbWclMjAyMDE3LTExLTE4XzEuanBnJmI9aG91emFqYmxvZw==)   
2. **如何处理四则运算优先级？**     
**①** 首先考虑到+-运算比×÷运算的优先级，所以可以开一个栈保存等待被+-运算的数，增加一个ch_save保存×或÷的运算符。每次读到×或÷运算符时，读取下一个数（或左括号），将该数与栈的头部元素作×÷运算后，压入栈中。    
![](http://houzajblog-1252277898.coscd.myqcloud.com/20171110%20PRERecusion/img%202017-11-18_2.jpg?sign=bsG4VZmlMJ5RXTO6EfYJSZGrDPNhPTEyNTIyNzc4OTgmaz1BS0lEVXVYME83aHpET1RSQ3Z2cWNJaHk5QzY3QjdLVGNSanEmZT0xNTEzNTkwODk0JnQ9MTUxMDk5ODg5NCZyPTE0NDY5NTg5NDcmZj0vMjAxNzExMTAlMjBQUkVSZWN1c2lvbi9pbWclMjAyMDE3LTExLTE4XzIuanBnJmI9aG91emFqYmxvZw==)   
**②** 另外，对于-号，可能存在`-(-3)`这种边缘数据，以及考虑到最后一次性将栈中的数取出作和运算更方便，因此将\'-\'号转换为(-1×)，即在栈中压入-1，将ch_save变为\'\*\'。      
![](http://houzajblog-1252277898.coscd.myqcloud.com/20171110%20PRERecusion/img%202017-11-18_3.jpg?sign=8ZMm9s1KdKZTJfll5U2HTWjA7zhhPTEyNTIyNzc4OTgmaz1BS0lEVXVYME83aHpET1RSQ3Z2cWNJaHk5QzY3QjdLVGNSanEmZT0xNTEzNTkwODk0JnQ9MTUxMDk5ODg5NCZyPTE3MTI2Mzg0MjMmZj0vMjAxNzExMTAlMjBQUkVSZWN1c2lvbi9pbWclMjAyMDE3LTExLTE4XzMuanBnJmI9aG91emFqYmxvZw==)     
**③** 由于最后栈中的元素之剩下和运算，遇到右括号\')\'或\'\#\'时，计算栈中的所有元素之和返回。      

**▲ 代码**   
``` cpp
//简易计算器
#include <bits/stdc++.h>  
using namespace std;

double calc(){
    stack<double> stk_num;
    char ch;
    char ch_save = '?';  
    double num;
    double sum = 0;
    for(;;){
        cin >> ch;
        if(isdigit(ch) || ch == '('){
            if(isdigit(ch)){
                ungetc(ch, stdin);  //将ch压回输入流中
                cin >> num;
            }
            if(ch == '('){
                num = calc();
            }
            if(ch_save != '?'){
                if(ch_save == '*'){
                    num *= stk_num.top();
                }else{
                    num = stk_num.top()/num;
                }
                stk_num.pop();
                ch_save = '?';
            }
            stk_num.push(num);
        }
        if(ch == ')' || ch == '#'){
            while(!stk_num.empty()){
                sum += stk_num.top();
                stk_num.pop();
            }
            return sum;
        }
        if(ch == '*' || ch == '/'){
            ch_save = ch;
        }
        if(ch == '-'){
            stk_num.push(-1);
            ch_save = '*';
        }
    }
}

int main()
{
    int t;
    cin >> t;
    while(t--){
        cout << fixed << setprecision(4) << calc() << endl;
    }
    return 0;
}
```

<br>

### 全排列（51NOD 1384）    
**▲ 题目简述**

> **Description**   
>> 给出一个字符串S（可能有重复的字符），按照字典序从小到大，输出S包括的字符组成的所有排列。例如：S = "1312"，
输出为：    
>>1123    
>>1132    
>>1213    
>>1231    
>>1312    
>>1321    
>>2113    
>>2131    
>>2311    
>>3112    
>>3121    
>>3211    

> **Input**    
>> 输入一个字符串S（S的长度 <= 9，且只包括0 - 9的阿拉伯数字）    

> **Output**    
>> 输出S所包含的字符组成的所有排列    

> **Sample Iutput**   
>> 1312   

> **Sample Output**    
>> 1123   
>> 1132   
>> 1213   
>> 1231   
>> 1312   
>> 1321   
>> 2113   
>> 2131   
>> 2311   
>> 3112   
>> 3121   
>> 3211    

**▲ 思路**    
![](http://houzajblog-1252277898.coscd.myqcloud.com/20171110%20PRERecusion/img%202017-11-18_4.jpg?sign=UaaH0zZCyvejBqzPEAoP+Vk3fsZhPTEyNTIyNzc4OTgmaz1BS0lEVXVYME83aHpET1RSQ3Z2cWNJaHk5QzY3QjdLVGNSanEmZT0xNTEzNjA1ODM2JnQ9MTUxMTAxMzgzNiZyPTQyNzgwNDg5OSZmPS8yMDE3MTExMCUyMFBSRVJlY3VzaW9uL2ltZyUyMDIwMTctMTEtMThfNC5qcGcmYj1ob3V6YWpibG9n)   
对数全排列的一般思维如图，另外题目中指明可能有重复字符，因此按递归生成的全排列可能会有重复，可以将生成的全排列数存入set中，利用set中元素的唯一性以及set中的元素是默认升序排序的，正好符合题目要求，AC题目！   
另外，STL中还提供了全排列函数next_permutation，可直接使用该函数AC题\_(\:з」∠)\_，需要注意的是，next_permutation只会生成字典序比它大的全排列数，因此输入数后需对其升序排序，用sort就可以了。     

**▲ 代码**   

```cpp
//全排列问题 —— 递归解
#include <bits/stdc++.h>
using namespace std;

void permutation(int from, int to, string str, set<string>& set_str){
    for(int i = from; i <= to; i++){
        swap(str[from], str[i]);    //将当前范围的第一个数与后面的数逐一交换回来
        if(from == to){
            set_str.insert(str);
        }else{
            permutation(from + 1, to, str, set_str);    //缩小范围
        }
        swap(str[from], str[i]);    //因为是string，记得交换回来，恢复原来的str
    }
}

int main()
{
    string str;
    cin >> str;
    set<string> set_str;
    permutation(0, str.length() - 1, str, set_str);
    set<string>::iterator itel_begin = set_str.begin();
    set<string>::iterator itel_end = set_str.end();
    for(; itel_begin != itel_end; itel_begin++){
        cout << *itel_begin << endl;
    }
    return 0;
}
```

```cpp
//全排列问题 —— next_permutation
#include <bits/stdc++.h>
using namespace std;

int main()
{
    string str;
    cin >> str;
    sort(str.begin(), str.end());

    do{
        cout << str << endl;
    }while(next_permutation(str.begin(), str.end()));
    return 0;
}
```
