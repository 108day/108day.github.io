---
title: github集成gitalk
tags: gitalk，github评论

---

# 概要
 本文主要讲解 gitalk 使用过程遇到的问题： Error: Not Found.

# 什么是gitalk

	这个网上很多，请自行百度google

## 集成gitalk的步骤

  ### 1. 引入 js，css
     省略
  ### 2. 添加脚本，html
    省略
 ###  3. 申请OAuth Apps（clientID 和 clientSecret）
    注意：Register a new OAuth application 页面时 ，Authorization callback URL 文本框是 你的github 的博客地址 （一般博客地址是 https://username.github.io ,比如我的是 https://konwworld.github.io )
### 4. 创建repositories 
    步骤：1. 右上角头像出进入 your repositories 页面
               2. 点击 Luangage:All 旁边的 New 进入 https://github.com/new
               3. Repository name 文本框中 填入 blog-comments 
               4. 选择Initialize this repository with a README
               5. 最后点击 Create repository 按钮 
               6. 完了
               
## 遇到的问题

1. 从头到尾就遇到了一个问题：
	页面提示 Error: Not Found. URL：
    https://api.github.com/repos/konwworld/blog-comments/issues?client_id=1f02192554bde9b07bf8&client_secret=bc467ba8d75e23cc041adc841401382b6d41aa28&labels=gitment,%2F2018%2F12%2F06%2Fmarkdonw.html&t=1544105334346 报错 404　

看了很多文章，结果都没有解决我的问题，知道看了一篇文章之后，知道要在　repositories　创建一个ropo的仓库 ，才知道因为自己没有创建这个仓库的原因。

2. 最后解决 方案就是 创建一个 ropo 同名的 repositories。

## 参考的文章

1. gitalk源码：https://github.com/gitalk/gitalk 
2. 最重要的一篇，解决了我的问题：https://www.cnblogs.com/laughitover/p/9069219.html 
3. 介绍了解gitalk 的文章 ：https://www.xttblog.com/?p=2792 
4. 还有其他的一些gitment ，disqus 的文章，也看了些，在这里因为不是用的这两个所以就不贴看过的文章了。

## 总结

		搞了好久了，真是假书传万卷，真经一句话啊！还是实践出真知，代码敲起了没有毛病。


