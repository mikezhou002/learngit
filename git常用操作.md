# Git 常用操作指南

## 理论
git 管理的是修改
工作区：电脑中的目录
版本库：工作区中的隐藏目录.git

## 基本操作

新建仓库：git init
查看仓库状态：git status

查看历史记录：git log
查看简短历史记录：git log --oneline
查看图形历史记录：git log --graph


修改提交到暂存区： git add <filename>
将所有修改提交到暂存区： git add .
将暂存区修改提交到当前分支：git commit -m 'message for this commit'

丢弃工作区，也即将暂存区或者分支中最近提交的内容覆盖工作区：git checkout -- <filename>
丢弃暂存区：git reset HEAD -- filename

删除文件：在文件管理器中删除文件 -> git rm <filename> -> git commit

回退版本：git reset --hard <abcde>
查看所有命令记录：git reflog


## 远程仓库
创建ssh密钥：ssh-keygen -t rsa -C "youremail@example.com"
GITHUB配置推送权限：登录GITHUB->账户设置->添加SSH KEY->将.ssh文件夹中id_rsa.pub内容复制后保存。
添加远程仓库：git remote add origin git@github.com:yourcount/test.git

本地内容推送到远程库(第一次推送，将本地master和远程master关联)：git push -u origin master
本地内容推送到远程库：git push origin master （推送其他分支：git push origin dev）
将远程仓库克隆到本地：git clone git@github.com:yourcount/test.git

https协议和ssh协议：https协议速度慢，且每次推送都需要输入密码

查看远程仓库信息：git remote
查看远程仓库更详细信息：git remote -v (如果看不到push，则没有push权限)

## 分支管理
分支的作用：多人合作时，在属于自己的分支上操作，不影响其他人。工作完成后，可以一次性合并到原分支。
HEAD的指向：HEAD其实不是指向提交，而是指向当前分支，例如master，每次提交，master前进一步。分支合并即是，master指针指向分支当前的提交。由于git创建和删除分支非常快，所以鼓励使用分支完成任务，合并后再删除分支。这和直接在master上工作效果是一样的，但是过程更安全。

创建并切换分支：git checkout -b dev (相当于： git branch dev; git checkout dev)
查看当前分支：git branch
合并分支：git merge dev
删除分支：git branch -d dev

switch新命令
创建并切换分支：git switch -c dev
切换到已有分支：git switch master

储藏当前工作现场：git stash
查看储藏的工作现场：git stash list
恢复工作现场：git stash apply (stash内容不删除)；git stash pop (恢复的同时删除工作现场)
删除工作现场：git stash drop

复制特定修改到当前分支：git cherry-pick <4c805e2>

强行删除没有合并的分支：git branch - D <name>

将本地没有push的分叉提交历史整理成直线：git rebase

本地创建远程相对应分支：git checkout -b branch-name origin/branch-name
建立本地分支和远程分支关联：git branch --set-upstream branch-name origin/branch-name

## 标签
标签的作用：有一个有意义的名字，是指向某个commit指针，与之绑定。
在最新commit上创建标签：git tag v1.0
查看所有标签：git tag
在指定commit上打标签：git tag v0.9 f52c633
查看指定标签信息：git show v0.9
创建带有说明的标签：git tag -a v0.1 -m 'version 0.1 released' 1094adb
删除标签：git tag -d v0.1
推送标签到远程仓库：git push origin v1.0
一次性推送全部尚未推送的本地标签：git push origin --tags
删除已推送的标签：先本地删除，git tag -d v0.9， 再远程删除，git push origin :refs/tags/v0.9