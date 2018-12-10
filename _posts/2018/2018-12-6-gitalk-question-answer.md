---
title: github集成gitalk
tags: Github,Gitalk,评论功能
titlebar: arch
author: 一百零八天

---

## 概要
本文主要讲解 gitalk 使用过程遇到的问题： Error: Not Found.
## 什么是gitalk
> Gitalk is a modern comment component based on GitHub Issue and Preact.
## gitalk的特征？

+ Authentication with github account
 需要github账号认证
+ Serverless, all comments will be stored as github issues 
<br>微服务的，所以评论信息都存储在github的issues
+ Both personal and organization github projects can be used to store comments
<br>个人和组织都可以使用github项目来存储评论信息
+ Localization, support multiple languages [en, zh-CN, zh-TW, es-ES, fr, ru]
<br>支持本地、多语言
+ Facebook-like distraction free mode (Can be enabled via the distractionFreeMode option)
 	<br>类似Facebook的<无分散模式>(可以通过distractionFreeMode<无分散模式>选项启用)
+ Hotkey submit comment (ctrl + enter)
	<br>快捷键提交评论（ctrl + enter）
## 集成gitalk的步骤

### 1、引入 js，css 
```
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
  <script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>
  <!-- or -->
  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
 ```

### 2、添加脚本，html 
#### 在你的页面需要评论的地方加上这个标签，用来展示评论的<br>
```
<div id="gitalk-container"></div>
```
#### 加载gitalk的javascirpt脚本插件
```javascript
   const gitalk = new Gitalk({
        clientID: 'GitHub Application Client ID',
        clientSecret: 'GitHub Application Client Secret',
        repo: 'GitHub repo',
        owner: 'GitHub repo owner',
        admin: ['GitHub repo owner and collaborators, only these guys can initialize github issues'],
        id: location.pathname,      // Ensure uniqueness and length less than 50
        distractionFreeMode: false  // Facebook-like distraction free mode
      }) 
      gitalk.render('gitalk-container')
```
### 3、申请OAuth Apps（clientID 和 clientSecret）

   在Register a new OAuth application 页面时 ,Authorization callback URL文本框是你的github 的博客地址 
  （一般博客地址是 https://xxxxxxx.github.io ,比如我的是 https://108day.github.io )

### 4 、创建repositories ( 与ropo参数同名)

	步骤：
        4.1 右上角头像出进入 your repositories 页面
        4.2. 点击 Luangage:All 旁边的 New 进入 https://github.com/new 
        4.3. Repository name 文本框中 填入 blog-comments  
        4.4. 选择Initialize this repository with a README 
        4.5. 最后点击 Create repository 按钮  
        4.6. 完了 
               
## 遇到的问题

### 问题描述

页面提示 Error: Not Found. URL：
    
URL报错 [404](https://api.github.com/repos/108day/blog-comments/issues?client_id=1f02192554bde9b07bf8&client_secret=bc467ba8d75e23cc041adc841401382b6d41aa28&labels=gitment,%2F2018%2F12%2F06%2Fmarkdonw.html&t=1544105334346 )
    
### 解决方案
最后解决方案就是创建一个 ropo 同名的 repositories。
看了很多文章，结果都没有解决我的问题，
直到看了[这篇文章](https://www.cnblogs.com/laughitover/p/9069219.html/) 之后，
知道要在repositories创建一个ropo名称的仓库 ，才知道因为自己没有创建仓库的原因。

## 参考的文章

- 1、gitalk源码：https://github.com/gitalk/gitalk
- 2、最重要的一篇，解决了我的问题https://www.cnblogs.com/laughitover/p/9069219.html
- 3、介绍了解gitalk 的文章 ：https://www.xttblog.com/?p=2792
- 4、还有其他的一些gitment ，disqus 的文章，也看了些，在这里因为用的是gitalk所以就不贴gitment ，disqus 的文章了。

## 总结

搞了好久了，真传一句话 假传万卷书！还是要实践才能出真知啊！


