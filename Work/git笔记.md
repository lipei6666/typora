# 1.为什么用git？



# 2.常用的场景



# 3.本地仓库使用教程

## 3.1设置用户和邮箱

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

## 3.2初始化仓库

```shell
$ git init
```

## 3.3添加文件到git仓库

```shell
#将文件加入缓存区
$ git add <file> 			#可重复使用添加多个文件

#将缓存区文件上传至仓库
$ git commit -m <message>	
```

## 3.4查看历史记录

```shell
$ git log  #输出信息太多可以加上 --pretty=oneline
$ git reflog #查看历史版本信息
```

## 3.5版本退回

git使用`HEAD`代表当前版本，上一个版本就是`HEAD~1`，上上版本就是`HEAD~2`，往上n个版本就是`HEAD~n`

```shell
$ git reset HEAD~1		  #默认参数，代表本地文件不改变，退回缓存区和仓库版本

$ git reset --soft HEAD~1 #退回仓库上一个版本，缓存区和本地源码不变

$ git reset --hard HEAD~1 #本地源码，缓存区，仓库都退回上一个版本

#知道log某一个版本的commit号，退回
$git reset --hard commitID
```

## 3.6查看仓库当前状态

```shell
$ git status
```

## 3.7撤销修改

```shell
#修改了本地文件，但没有上传到缓存区
$ git checkout -- <file>  #可以删除本地文件的修改

#修改了本地文件后，上传到缓存区，然后又再一次修改了本地文件
$ git checkout -- <file>  #可以删除本地文件的第二次修改

#修改了本地文件后，上传到缓存区，想撤销修改
$ git reset HEAD <file>   #撤销缓存区的修改
$ git checkout -- <file>  #撤销本地文件的修改
```

## 3.8删除文件

```shell
#删除仓库文件
$ git rm <file>

#误删了本地文件，但仓库还有
$ git checkout -- <file> #恢复本地文件
```

## 3.9查看修改内容

```shell
#查看具体到每个commit修改内容
$git log -p

#当前工作区与历史某一个版本之间的改动
$git diff 某个版本的commitID .
```





# 4.远程仓库

## 4.1创建SSH秘钥

```shell
$ ssh-keygen -t rsa -C "youremail@example.com"
```

打开`.ssh/id_rsa.pub`文件内容填入网站SSH公钥中

## 4.2绑定远程仓库

```shell
$ git remote add [shortname] [url]

$ git remote -v #查看绑定远程仓库信息
```

## 4.3发布

```shell
$ git push shortname <分支名>
```

## 4.4从远程仓库获取最新版本

```shell
$ git fetch shortname master

$ git pull
```

## 4.5比较远程仓库和本地仓库区别

```shell
$ git merge master
```





# 5.分支管理

## 5.1创建分支

```shell
#方法1
$ git checkout -b <name>  #创建并切换分支

#方法2
$ git switch -c <name>    #创建并切换分支
```

## 5.2查看当前分支

```shell
$ git branch
```

## 5.3切换分支

```shell
方法1
$ git chekout <name>

方法2
$ git switch <name>
```

## 5.4合并分支

```shell
$ git merge <name> #合并某分支到当前分支 加上参数--no-ff能保存历史合并信息
```

## 5.5删除分支

```shell
$ git branch -d <name>

$ git branch -D <name> #强行删除未合并的分支
```



```shell
#main中有一个名为test.txt的文件
#创建一个名为dev的分支
$ git checkout -b dev

#对test.txt进行修改后需要添加到缓冲区和仓库，此时切回mian分支test.txt没有发生改变
#如果没有提交到仓库，切换回main分支会发生改变

```

## 5.6分支储藏

当前分支进行还没有完成时，又不想提交到仓库，可以使用分支储藏，然后切换成其他分支干别的事

```shell
$ git stash  		#储存
$ git stash list	#查看储存信息
$ git stash pop		#恢复现场
```



# 6.标签管理

```shell
$ git tag <name>  #打标签
$ git tag         #查看标签
```

