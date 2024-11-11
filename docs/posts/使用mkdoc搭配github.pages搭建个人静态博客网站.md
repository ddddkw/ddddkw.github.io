---
date: 2024-11-11
title: 使用mkdocs搭配github.pages搭建个人静态博客网站
year: 2024
author: 豆凯威
---

**记录mkdocs的使用过程**

整个过程的前提是需要拥有一个github账号（这个对各位开发者们应该是一件简简单单的事情，就不赘述了）

然后是需要进行环境配置，安装Mkdocs是需要配置python环境的

环境配置好以后执行以下代码安装Mkdocs：

```
 pip install mkdocs
```

为了使网页更加的好看，可以安装Materia For MkDocs，使用下面的指令进行安装：

```
 pip install mkdocs-material
```

###### 仓库创建

我们需要在github下创建一个仓库，注意仓库需要为公共的

后续创建项目，连接远程仓库后，push到远程仓库编译后，即可通过云端服务器网址进行访问

###### 创建Mkdocs项目

在上文我们安装好Mkdocs以后，可以执行以下命令很快的开始一个简单的项目：

```
$ mkdocs new my-project
$ cd my-project
```

创建完成以后，运行mkdocs serve命令就可以看到本地运行的地址，浏览器中打开就可以看到一个简易的Mkdocs项目

```
$ mkdocs serve
Running at: http://127.0.0.1:8000/
```

