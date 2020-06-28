---
layout:     post
title:      LibraryWeb
subtitle:   
date:       2019-03-01
author:     QK
catalog: true
tags:
    - Markdown
    - JavaWeb
    - MVC
---

<!-- MarkdownTOC -->

​	完成一个B/S架构的数据库管理系统 (Window/图形界面) 完成图书资料的管理，处理的信息包括图书信息、读者信息、出版社、图书分类、图书借阅等





## 整体架构：

##### 操作主体：

1. 学生：登陆（自动登陆）、注册、检索借书、还书、修改个人信息、退出登录

2. 管理员：

   a)  登陆、注册、退出登录

   b)  添加图书、按（类，名、出版社、作者）检索图书、删除丢失图书、修改图书信息

   c)  查看学生借阅列表（申请丢失）、查看超时列表（申请丢失）、修改学生信息

##### 数据主体：

1. 学生表：ID（学号），姓名，性别，登录密码，信用值
2. 书籍表：ID，书名，作者，出版社，分类，书的状态
3. 管理员表：ID，密码

## MVC架构

##### 概述：

MVC模式(Model–view–controller)是软件工程中的一种软件架构模式，把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller）。

模型（Model） 用于封装与应用程序的业务逻辑相关的数据以及对数据的处理方法。“ Model ”有对数据直接访问的权力。

视图（View）能够实现数据有目的的显示（理论上，这不是必需的）。在 View 中一般没有程序上的逻辑。为了实现 View 上的刷新功能，View 需要访问它监视的数据模型（Model），因此应该事先在被它监视的数据那里注册。

控制器（Controller）起到不同层面间的组织作用，用于控制应用程序的流程。它处理事件并作出响应。“事件”包括用户的行为和数据 Model 上的改变。

##### 图书管理MVC实现构思：

数据库管理：JDBC中分为四个模块：

MODEL:

- Entity:数据模块，定义数据类型，与数据库的表相对应

- Dao:方法模块，实现数据库的操作与管理的方法集合

- Util: 连接模块，实现程序与数据库的连接用于直接操作数据库

VIEW:

- Web:网页模块，实现数据库的管理

CONTROLLER:

- Servlet：交互模块，实现前端后台的链接与信息交换

##### 图书管理系统的交互与划分

###### 按结构划分：

content

- Entity :与数据库关系相对应，保存到文件为student，book，robot，worker；
- 为实现特殊操作保存视图stuview；
- Dao :保存数据修改操作方法：共保存在学生操作stuOperater,教师操作worOperater类中；
- Util: 保存JDBC连接驱动；

Web：

- JSP文件来实现前端的显示与交互
- CSS文件实现前端界面的设计；
- Servlet:与学生操作及管理员操作相对应建立两个包，保存学生操作与管理员操作各个交互实现；

<div align="center">
 <img src="C:\Code\Blog\qk88.github.io\img\javaweb.png" style="zoom:60%;" />
</div>

