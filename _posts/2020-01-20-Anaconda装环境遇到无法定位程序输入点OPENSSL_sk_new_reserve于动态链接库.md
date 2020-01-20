---
layout:     post
title:      Anaconda装环境遇到无法定位程序输入点OPENSSL_sk_new_reserve于动态链接库...
subtitle:   
date:       2020-01-20
author:     MoYu
catalog: true
tags:
    - Anaconda
    - Python


---

<!-- MarkdownTOC -->

办法总比困难多！

尝试了多种方法后，终于解决了问题。多数人的解决方案也不一定就是对的，适合自己的才是最好的！









#### Anaconda新建虚拟环境报错

win10 环境安装的anaconda3，在新建虚拟环境时出现如下类似错误：

```
(base) C:\Users\x>conda create -n torchEnv  python=3.6
Collecting package metadata (current_repodata.json): failed

CondaHTTPError: HTTP 000 CONNECTION FAILED for url <https://repo.anaconda.com/pkgs/main/win-64/current_repodata.json>
Elapsed: -

An HTTP error occurred when trying to retrieve this URL.
HTTP errors are often intermittent, and a simple retry will get you on your way.

If your current network has https://www.anaconda.com blocked, please file
a support request with your network engineering team.

SSLError(MaxRetryError('HTTPSConnectionPool(host=\'repo.anaconda.com\', port=443): Max retries exceeded with url: /pkgs/main/win-64/current_repodata.json (Caused by SSLError("Can\'t connect to HTTPS URL because the SSL module is not available."))'))
```



**搜索解决方案，说可以把源的`https`改成`http`**，即：

找到C:\Users\x\.condarc，用Notepad++编辑，把原来的`https`换成了`http`
[^_^]:![图1]({{sit.baseurl}}/img/image-20200120191154454.png)




于是不再报错，然而在下载packages的时候，又碰到如下错误：

[^_^]:![图1]({{sit.baseurl}}/img/image-20200120191154454.png)




很多博客说只需要做如下操作：

>出现上述问题 查看Anaconda\DLLS目录下和Anaconda\Library\bin下的libssl-1_1-x64.dll 查看修改日期，两个不一致 解决方法是用DLLS下的一个替换掉另一个（**不要反过来替换！**），替换之前可先备份一下，问题可解决



### 然而，在Anaconda\DLLs目录下根本就没有libssl-1_1-x64.dll这个文件！！

（于是绝望了，经历了一次卸载重装，但还是报同样的错）



最后终于在Google的帮助下，找到了如下解决方法：

**解决方法**

> 把路径“Anaconda3/Library/bin ”下面的文件复制到路径“Anaconda3/DLLs”下 :
> libcrypto-1_1-x64.dll
> libssl-1_1-x64.dll

该解决方法来自：

[【环境配置】Windows10系统下VSCode和Anaconda的配置和安装](https://www.cnblogs.com/saiminhou/p/12164109.html)