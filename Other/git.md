# Git笔记



git是一款分布式版本控制系统.

## Git的安装

*   Linux

    ```commend
    git 查找本地是否已经安装git
    sudo apt-get install git 安装git(Ubuntu或者debian)
    ```

*   Windows

    从git官网下载安装包进行安装

**安装完成后需要设置用户名和邮箱**

```git
$ git config --global user.name "Your Name"
$ git config --global user.email "YourEmail@example.com"
```



## 创建Git版本库

*   什么是版本库(repository)

    版本库是一个存储所有被git管理的文件的目录,每个文件的修改,删除都可以被Git跟踪,以便于任何时刻都可以追踪历史,或者还原历史.

*   创建一个版本库

    切换到需要建立版本库的目录下,**Windows下目录不能包含中文**

    ```git
    $ git init
    ```

    该目录便变成一个可以被Git管理的仓库

## 向版本库添加文件

*   在版本库目录或者子目录下创建文件 **example.txt**

    ```example.txt
    hello git!
    ```

*   使用 **git add …** 把文件添加到版本库

    ```git
    $ git add example.txt
    ```

*   使用 **git commit** 把文件提交到仓库

    ```git
    git commit -m "这里是提交仓库的注释"
    ```

## 查看Git仓库的状态&文件差异

​	使用 **git status** 可以查看仓库当前状态

​	使用 **git diff …** 命令可以查看文件的差异

## Git 版本回滚

*   使用 **git log** 查看Git仓库的提交记录

    ```git
    $ git log
    ```

*   进行版本回滚

    *   在版本回滚之前需要知道Git仓库当前的版本是哪一个版本,在Git中,用**HEAD** 表示当前的版本,上一个版本就是**HEAD^** ,再上一个版本就是**HEAD^^**, 前n个版本就是**HEAD~n**
    *   使用 **git reset —head commit_id** 回滚到指定的版本

    ```git
    $ git reset --head HEAD^
    ```

*   恢复版本库到回滚之前的版本

    *   使用 **git reflog** 查看历史指令(查看指令的commit_id)
    *   使用 **git reset —head commit_id** 回到指定的版本

*   使用Git撤销修改

    *   使用 **git checkout — \<file\>** 恢复暂存区文件到上一次**git commit**或者**git add**的状态

*   使用Git删除仓库中的文件

    *   当在本地删除文件后,通过 **git status** 可以查看到文件以被删除如果要提交删除文件的操作,可以使用 **git rm \<file\>** 

    ```git
    $ git rm example.txt
    $ git commit
    ```


