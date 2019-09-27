---
layout: post
title: '计 · Mkdocs with Mathjax Export PDF'
date: 2019-09-27
author: hpp2334
cover: 'https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190927%20Skill_Mkdocs_with_Mathjax_to_pdf/main-01.png'
tags: Skill
---

> 本文给出了带有MathJax的Mkdocs项目导出PDF的一种方法

&nbsp;

### 背景
近日HouZAJ委托了个人将OI-WIKI转为一个PDF文档。  
OI-WIKI是一个Mkdocs项目，另有动态加载的公式，使用MathJax渲染。目前（请注意发文时间），Mkdocs官方没有提供Mkdocs项目导出PDF的功能，需要用插件或别的方法实现导出pdf。  
经过半周的各种踩坑，终于将其转为PDF文档。  

&nbsp;

### 环境
- OS  
  - Windows 10  
  - Manjaro Linux  
- Project
  - OI-WIKI ([https://github.com/OI-wiki/OI-wiki/](https://github.com/OI-wiki/OI-wiki/))  
- Windows Application  
  - Chrome  
  - Adobe Acrobat DC  
  - AutoBookmark™ Plug-in for Adobe® Acrobat®  
- NodeJS  
  - mathjax-node  
  - mathjax-node-page  
- Linux Application
  - pandoc
  - mkdocs-combine

&nbsp;

### 步骤
#### Step I. 安装与配置环境
此步略，简单地说，该git就git，该npm就npm，该pip就pip。  

#### Step II. 组合Mkdocs为一个单文件，并修改此文件
在oi-wiki项目下，执行：  
```git
mkdocs build -v
mkdocscombine -o mydocs.pd
```
这样能够得到一个`mydocs.pd`文件，用文本编辑器打开（尽量不要用Windows自带的记事本），会发现其为Markdown文件，但是将这个OI-WIKi整合后的结果。  
值得注意的是，需要修改此文件中的 **图片路径**，使其能够正确指向图片文件位置。  

#### Step III. 将mydocs.pd文件转为HTML
将`mydocs.pd`改名为`index.md`，覆盖根目录下的`index.md`文件，后在项目中执行：  
```git
mkdocs build -v
```
这样得到的`index.html`即为组合后的html文件。  

#### Step IV. 静态渲染公式
**内存足够大的用户或许可以跳过这一步。**  
实测发现，如果此时直接将`index.html`使用Chrome的导出PDF功能，占用内存过大可以直接导致Chrome崩溃。因此，使用`script`目录下的`render_math.js`渲染HTML中的公式，如：  
```git
node --max-old-space-size=4096 ./render_math.js ./index.html
```
其中，`--max-old-space-size=4096`指明了`node`最大可用的内存为4096MB。  

#### Step V. 将HTML转为PDF文件
使用Chrome的打印功能，将`index.html`保存为PDF格式。  
在个人环境中，Chrome的内存占用峰值为3GB。  

#### Step VI. 生成目录
使用Adobe Acrobat的AutoBookmark插件先生成书签，再生成目录，这些功能这一插件都有提供。  

&nbsp;

### Q & A
#### 为什么不使用MkDocs PDF Export Plugin
这一插件基于WeasyPrint，而WeasyPrint不加载任何JS文件，因此对于未经过服务端渲染的MathJax公式，其无法显示。  
#### 为什么不使用Pandoc将mydocs.pd转为pdf
如果使用这种做法，latex引擎若为pdflatex，不支持unicode字符（中文为unicode字符），而若为xelatex，则会出现一个错误，这一错误需要latex引擎设置为pdflatex解决。  
个人无法解决这个问题，因而放弃了这种做法。  


![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190721%20Avatar_hpp2334/main-01.png)
