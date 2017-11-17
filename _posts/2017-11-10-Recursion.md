---
layout: post
title: '递归 (Recursion) —— PPT 与 实例'
date: 2017-11-10
author: HouZAJ
cover: ''
postPatterns: 'circuitBoard'
tags: Programming
---

> 其实，这是一篇试iframe的文...       
> （2017.11.17）更新了一些实例

<br>

<iframe type="text/html" src="http://music.163.com/outchain/player?type=2&id=28492953&auto=0&height=66" frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86"></iframe>
### PART I - 前言
哈哈哈这次PRE，emmmm……严！重！超！时！

<br>
### PART II - 大计演讲PPT
<iframe class="iframe-ppt" src='https://view.officeapps.live.com/op/embed.aspx?src=http%3A%2F%2Fhouzajblog%2D1252277898%2Ecoscd%2Emyqcloud%2Ecom%3A80%2F20171110%2520PRERecusion%2FRecursion%2Epptx%3Fsign%3D7A2HAkwEFgn%2FlnWztUfyu5U3vbhhPTEyNTIyNzc4OTgmaz1BS0lEVXVYME83aHpET1RSQ3Z2cWNJaHk5QzY3QjdLVGNSanEmZT0xNTEyOTA3MjI4JnQ9MTUxMDMxNTIyOCZyPTU1MzUwNTQ4NCZmPS8yMDE3MTExMCUyMFBSRVJlY3VzaW9uL1JlY3Vyc2lvbi5wcHR4JmI9aG91emFqYmxvZw%3D%3D&wdAr=1.3333333333333333'  frameborder='0'>This is an embedded <a target='_blank' href='https://office.com'>Microsoft Office</a> presentation, powered by <a target='_blank' href='https://office.com/webapps'>Office Online</a>.</iframe>

PPT下载： [https://mega.nz/#F!tXADUZjI](https://mega.nz/#F!tXADUZjI!Si0jjfq0RQDEvS2PKKnO5Q)
<br>

### PART III - 实例
#### 简易计算器
- 题目简述      
> **Input**    
>> 输入T，表示有T个实例      
>> 接下来每行输入一个表达式，每个表达式末尾带#表示结束   

> **Output**    
>> 输出结果，保留四位小数        

> **Sample Iutput**   
>> 2    
>> 1+2*3-4/5#    
>> (66+(((11+22)*2-33)/3+6)*2)-45.6789#   

> **Sample Output**    
>> 6.2000        
>> 54.3211      
- 思路    
* **如何处理 左括号'(' 和 右括号')' 呢**       
（这里先感谢一下俊生大佬！）受俊生大佬启发，对于括号内的部分，其实和运算整体使用的方法是一样的，因此遇到左括号'('可以调用自身函数，直到遇到右括号')'结束自身调用（即返回结果）。另外遇到'#'号也应该返回结果（给主函数）。
* 
- 代码
<pre class="line-numbers"><code class="language-cpp">//简易计算器
#include "bits/stdc++.h"
using namespace std;

double calc(){
    stack<double> stk_num;
    char ch;
    char ch_save = '?';  //储存*或/的运算符
    double num;
    double sum = 0;
    for(;;){
        cin >> ch;
        if(isdigit(ch) || ch == '('){
            if(isdigit(ch)){
                ungetc(ch, stdin);  //
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
} </code></pre>
