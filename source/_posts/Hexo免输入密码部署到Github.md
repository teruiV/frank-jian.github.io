---
title: Hexo免输入密码部署到Github
date: 2016-04-14 12:37:31
categories: Hexo
tags: [Hexo]
---
在使用hexo deploy命令部署hexo到github时，每次都要输入用户名和密码，这种重复机械的流程，让人感觉很烦躁。以下是介绍如何解决这个问题：

1、在系统环境变量中新增一个环境变量
![](/images/env.png)
2、接着在你的用户目录(C:\Users\username)下新建一个叫_netrc的文件，在文件中添加以下内容：
```
machine github.com
login username //username为github账户名 （ps：请将注释内容去掉）
password password //password为github账户的密码
```
3、设置好这些之后，当你再次部署时，就不用再输入用户名和密码了。

参考资料：
[Hexo免输入密码部署到Github](http://www.foreverpx.cn/2014/09/25/Hexo%E5%85%8D%E8%BE%93%E5%85%A5%E5%AF%86%E7%A0%81%E9%83%A8%E7%BD%B2%E5%88%B0github/)