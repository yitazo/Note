# Git

## 基础命令

初始化仓库：git init

创建文件：touch 文件名

删除文件：rm 文件名

vi编辑器：i进入编辑，esc推出编辑，输入:号wq保存退出，q!强制不保存退出

工作目录：除了.git，其它都是工作目录，创建了一个文件，状态是未跟踪，修改了已有文件，状态是未暂存。使用git add .让工作区进入暂存区，使用git commit让暂存区进入本地仓库

查看工作区状态：git status

查看日志：git log或者自定义的git-log

回退版本：git reset --hard commitID，这个id可以在git log里面查看

清空屏幕：clear

查看历史操作：git reflog，通过分析可以找到想要的commitID

## 忽略文件

先创建.gitignore文件，然后在里面输入哪些文件不需要git来管理

*.xml代表所有.xml文件都不要git管理

## 分支

查看有哪些分支：git branch

新创建分支：git btanch 分支名

切换分支：git checkout 分支名	

创建并切换到分支：git checkout -b 分支名

合并分支：git merge 分支名，将分支名合并到当前分支

删除分支：git branch -d 分支名，做各种检查再删除，-D不检查强制删除

查看本地分支与远程分支关系：git branch -vv

## 远程仓库

生成公钥：ssh-keygen -t rsa，然后一路回车，如果已有公钥会自动覆盖，远程仓库软件配置ssh，然后使用ssh -T git@gitee.com或者git@github.com，然后输入回车

添加远程仓库：git remote add origin 仓库ssh，然后使用git remote查看已有的远程仓库

本地仓库推到远程仓库：git push -f --set-upstream origin master:master，-f强制覆盖远程内容一般不用，后面那个master是远程的分支名，如果一样可不写

--set-upstream将当前分支与远程分支绑定，下次再推只使用git push即可完成

