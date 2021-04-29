---
title: git操作

date: 2019/3/16 21:10:43

updated: 2019/3/16 21:10:43

categories:
 - git操作
 
tags:
 - git操作
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)

## 说明：
![git-command.jpg](https://i.loli.net/2021/04/07/hR3zFJm9A4svVcp.jpg)

- workspace：工作区

- staging area：暂存区/缓存区

- local repository：版本库或本地仓库

- remote repository：远程仓库

  

  ##  创建仓库命令

  | 命令      | 说明                                   |
  | --------- | -------------------------------------- |
  | git init  | 初始化仓库                             |
  | git clone | 拷贝一份远程仓库，也就是下载一个项目。 |

  ##  提交与修改

  | 命令       | 说明                                     |
  | ---------- | ---------------------------------------- |
  | git add    | 添加文件到仓库                           |
  | git status | 查看仓库当前的状态，显示有变更的文件。   |
  | git diff   | 比较文件的不同，即暂存区和工作区的差异。 |
  | git commit | 提交暂存区到本地仓库。                   |
  | git reset  | 回退版本。                               |
  | git rm     | 删除工作区文件。                         |
  | git mv     | 移动或重命名工作区文件。                 |

  ##  提交日志 

  | 命令            | 说明                                 |
  | --------------- | ------------------------------------ |
  | git blame<file> | 查看历史提交记录                     |
  | git log         | 以列表形式查看指定文件的历史修改记录 |

  ## 远程操作

  | 命令     | 说明         |
  | -------- | ------------ |
  | git pull | 下载远程代码 |
  | git push | 上传远程代码 |

  ## 分支管理

  | 命令                      | 说明             |
  | ------------------------- | ---------------- |
  | git branch                | 列出分支基本命令 |
  | git branch (branchname)   | 创建分支         |
  | git checkout (branchname) | 切换分支         |
  | git merge                 | 合并分支         |

  ## git rebase master和git merge dev 区别

  

  git rebase master   只保留被合并分支的历史   是线性得  所有得平行得都不会被保留
  git merge dev  保留所有的提交   方便代码回退

  ## 合并冲突

  

  当合并有冲突  两个分支都进行了修改   删掉本地的标识  重新提交

  
  
  