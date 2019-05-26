---
layout: post
title: '蒟蒻的算法PPT - 最近公共祖先（LCA）的倍增求法 '
date: 2019-05-25
author: HouZAJ
cover: 'https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190525%20ALG-PPT-RMQ/%E5%B9%BB%E7%81%AF%E7%89%871.PNG'
tags: Algorithm PPT
---

> CONTENT: 蒟蒻的算法PPT - 最近公共祖先（LCA）的倍增求法  
> DETAIL: RMQ  

<br>

### BB
本来今天想写DP的文的，然后就被作业拖死了 T^T  
不说了发完接着写作业 T^T  
欢迎各位dl找bug QAQ（蒟蒻还是太菜了 QvQ  
<br>

### PPT
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%871.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%872.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%873.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%874.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%875.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%876.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%877.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%878.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%879.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8710.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8711.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8712.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8713.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8714.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8715.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8716.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8717.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8718.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8719.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8720.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8721.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8722.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8723.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8724.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8725.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8726.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8727.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8728.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8729.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8730.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8731.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8732.PNG)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8733.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8734.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8735.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8736.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8737.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8738.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8739.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8740.PNG)   ![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190526%20ALG-PPT-LCA_%E5%80%8D%E5%A2%9E/%E5%B9%BB%E7%81%AF%E7%89%8741.PNG)  
