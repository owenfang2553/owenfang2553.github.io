---
title: Git总结
date: 2018-4-10 23:00:07
tags: Git
categories: Git
---
## 概念理解
* index暂存(又名staging area--暂存区)

         暂存区是可以设置哪些变更要提交到版本库，哪些先不提交。临时存放的地方（staging area）。
* work area--工作区

        我们工作的区域空间
* local repository--本地仓库

        就是我们自己工作的电脑上保存版本数据的地方
* remote repository--远程仓库

        我们用Git进行操作，为了防止数据在自己电脑上丢失，比如错误删除，病毒攻击等原因造成了数据丢失，我们需要备份到远程的服务器上，这个服务器可以理解为远程仓库。
## 生成SSH Key
1. 在用户目录下查看是否有.ssh目录若有则查看目录下是否有id_rsa（密钥）和id_rsa.pub（公钥）若有则不必再次生成直接使用
2. 创建SSH Key：ssh-keygen -t rsa -C "851580041@qq.com"
3. 登录到代码托管平台(如github)打开账户设置中的SSHKey页面粘贴公钥
4. 可添加多个key(只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了)
## 使用流程
    [workspace] -> add -> [local cache] -> commit ->[local repository] -> push ->[remote git repository]-> clone -> [git repository] checkout -> [workspace]
## 常用操作
### 本地初始化关联远程仓库
```
git config --global user.name 'nick'
git config --global user.email '851580041@qq.com'
git init gitlearn # 初始化项目
git status # 查看状态
git add 1.txt # 添加修改到本地缓存
git add -A # 添加所有到本地缓存
git commit -am '1.txt' # 添加提交到本地仓库
git remote add origin https://github.com/csy512889371/gitlearn.git #添加远程仓库
git remote #查看远程
git push origin master -u
```
### 克隆及更新
```
git clone https://github.com/example/example.git #克隆项目
git pull # 拉取代码
```
### 分支管理
#### 查看切换合并
```
git branch [-v] # 查看当前分支
git branch <branch name># 基于当前分支新建分支
git branch <branch name> <commit id># 基于提交新建分支
git checkout <branch name> #切换分支
git merge <merge target> #用于合并指定分支到当前分支

# 解决冲突，如果因冲突导致自动合并失败，此时status为mergeing状态
# 需要手动修改后重新提交(commit)
```
#### 创建删除分支
```
git branch
git branch -a
git branch dev #创建分支dev
git checkout dev #切换到dev分支
git branch -d dev #删除分支
git branch -D 强行删除未合并的分支
git push origin dev -u #将分支提交到远程服务器

git branch -av
git branch -avv
```
#### 提交代码冲突
```
git pull
# 本地合并
git commit -am '重新提交'
git push #提交到服务器
```
#### 如果本地项目和远程都有项目且未做关联
```
git branch --set-upstream-to=origin/master master
git pull --allow-unrelated-histories
```
#### 合并时强制禁用Fast forward
```
首先，仍然创建并切换dev分支：
git checkout -b dev
修改readme.md文件，并提交一个新的commit：
git add readme.md
git commit -m "add merge"
现在，我们切换回master：
git checkout master
准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
git merge --no-ff -m "merge with no-ff" dev

git merge –no-ff 可以保存你之前的分支历史。能够更好的查看 merge历史，以及branch 状态。
git merge 则不会显示 feature，只保留单条分支记录。
```
### 标签
```
git tag v1.0 #创建标签
git tag #标签状态
git push origin v1.0 -u #提交标签到远程仓库

git tag -d v1.0 #删除标签
git branch v1.0_dev v1.0 #基于标签创建分支
git log #查看日志
```
#### 标签管理（标签也是版本库的一个快照）
```
命令git tag <tagname>用于新建一个标签，默认为HEAD，也可以指定一个commit id。
git tag -a <tagname> -m "blablabla..."可以指定标签信息。
还可以通过-s用私钥签名一个标签：
git tag -s v0.5 -m "signed version 0.2 released" fec145a
git tag可以查看所有标签。
用命令git show <tagname>可以查看某个标签的详细信息。
如果标签打错了，也可以删除：
git tag -d v0.1
因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。
如果要推送某个标签到远程，使用命令git push origin <tagname>：
git push origin v1.0
或者，一次性推送全部尚未推送到远程的本地标签：
git push origin --tags
如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
git tag -d v0.9
然后，从远程删除。删除命令也是push，但是格式如下：
git push origin :refs/tags/v0.9
```
### 项目目前状态
```
$git status//查看当前git版本库的状态
```
### 退回某个版本步骤
1. git log查看所有历史版本
2. 获取某个历史版本的id
3. git reset --hard id值
4. 将修改提交到服务器（git push -f -u origin master）
### 存储当前工作状态
1. git stash （储藏当前状态之后，就能切换到别的分支）
2. git stash list (查看储藏状态的列表)
3. git stash apply 储藏的名字 （回到原来的分支之后，如何恢复到之前那种混乱的工作状态）
### 实际开发中版本控制
#### 原则
1. master与daily-test为只读分支
2. 所有合并原则上不可逆
3. 日常或紧急分支上线后就不可再使用（建议删除）
4. master与线上完全同步
5. daily-test版本必须大于等于master
![img1](https://github.com/csyeva/eva/raw/master/img/github/bb1.png)
![img1](https://raw.githubusercontent.com/csyeva/eva/master/img/github/bb2.png)
