# 本地代码连接多个远程仓库

今天开发时遇到了一个情况，将一个系统A的一个大模块单独抽离出来做一个系统B。这样遇到一个问题，当同样的东西在系统A内修改过后，难道还要在B的代码中再cv一遍？这样不免有点麻烦，后面发现有其他解决方案。

主要思路是，在B的代码仓库中创建一个中间分支，用与存储A的主分支的代码。在A的代码中修改以后，通过Cherry-Pick将指定的commit合并到B的分支上，这样可以最小程度的合并需要的代码。

首先需要将B的本地代码同时与A的远程仓库也建立连接

```
git remote add target http://xxx.xxx.xxx.xxx:xxxx/web/admin.git
```

查看本地代码关联的远程仓库

origin是源仓库地址，target是另外一个目标仓库地址

```
origin  http://xxx.xxx.xxx.xxx:xxxx/web/admin.git (fetch) 
origin  http://xxx.xxx.xxx.xxx:xxxx/web/admin.git (push)
target  http://xxx.xxx.xxx.xxx:xxxx/web/admin1.git (fetch)
target  http://xxx.xxx.xxx.xxx:xxxx/web/admin1.git (push)
```

然后本地建立一个中间分支

```
git checkout -b middle-branch
```

切换middle-branch后，拉取A中的主分支的代码

```
git pull target release  --allow-unrelated-histories // 允许两个没有共同历史记录的分支进行合并
```

