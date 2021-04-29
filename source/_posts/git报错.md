---
title: git报错

date: 2018/7/15 10:46:25

updated: 2018/7/15 10:46:25

categories:
 - Git
 
tags:
 - Git
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)

## git错误 error: failed to push some refs to 'https://github.com/...
**问题描述**

在git bash中键入 $ git push origin master 进行提交的时候出现 如下错误
![20480987-8dbab7b786177a26.png](https://i.loli.net/2021/03/22/mzNKwGx2LjU37ch.png)
**出现原因**

远程库与本地库不一致造成的，在hint中也有提示把远程库同步到本地库就可以了

**解决办法**

使用命令行
>git pull --rebase origin master

该命令的意思是把远程库中的更新合并到（pull=fetch+merge）本地库中，–-rebase的作用是取消掉本地库中刚刚的commit，并把他们接到更新后的版本库之中,执行成功后，可以成功执行git push origin master操作

## git pull 拉取更新报错，error: Cannot pull with rebase: You have unstaged changes.

**git pull --rebase origin master，直接拉取，有时候会报错如下**
```
error: cannot pull with rebase: You have unstaged changes. 
error: please commit or stash them.
```
这是因为本地有更改没有提交

如果需要提交，就git add ,git commit,提交上去；

如果不需要提交更改，就暂存
>git stash，

再执行 即可获取更新 
>git pull --rebase origin master



