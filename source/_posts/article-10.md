---
title: git命令总结
author: Robbin
banner_img: /images/common/default_index_img.jpg
index_img: /images/common/git.jpeg
date: 2022-03-08 16:15:03
updated: 2022-03-08 16:15:03
tags: ["#Git", "#前端"]
categories: Git
---

创建本地仓库

mkdir 创库名字
cd 仓库名字

pwd #查看仓库路径

git init #把目录变成 Git 可以管理的仓库

### 1.创建一个 git 版本库

```
mkdir github
cd github
pwd
/f/github
```

Mkdir 是新建一个文件夹（github）
cd 进入 github 目录
pwd 命令用于显示当前目录。在我的电脑上，这个仓库位于/f/github。

通过 git init 命令把这个目录变成 Git 可以管理的仓库：

```
git init
```

瞬间 Git 就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git 的目录，这个目录是 Git 来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把 Git 仓库给破坏了。
如果你没有看到.git 目录，那是因为这个目录默认是隐藏的，用 ls -ah 命令就可以看见。

### 2.上传项目到 github

```
#1.上传到工作区
git add .
#2.填写更新信息
git commit -m ‘配置管理’
#3.拉取GitHub源代码
git pull
#4. 上传项目
git push
```

### 3.分支命令

#### 1.查看分支：

```
git branch  # 查看本地分支
git branch -r  # 查看远程分支
```

#### 2.创建分支：

```
git branch <name>
```

#### 3.切换分支：

```
git checkout <name>
```

#### 4.创建+切换分支：

```
git checkout -b <name>
```

#### 5.创建本地分支并拉取远程分支：

```
git checkout -b 本地分支名 origin/远程分支名
```

#### 6.如果 5 命令行执行失败，则执行此命令更新远程然后再执行 5 命令行：

```
git fetch
```

#### 7.本地分支和远程分支关联：

```
git branch --set-upstream-to=origin/dev dev
```

#### 8.合并某分支到当前分支：

```
git merge <name>
```

#### 9.删除分支：

```
git branch -D <name> // 删除本地分支
git push origin :<name>   // 删除远程分支
// 刚提交到远程的test将被删除，但是本地还会保存的，不用担心
```

#### 10.查看仓库地址

```
git remote -v  // 查看链接的远程仓库地址
```

#### 11.跳过代码检查

```
git commit -m "注释" -n // 跳过代码检查，避免因为钩子无法提交代码
```

`git status`命令用于显示工作目录和暂存区的状态。使用此命令能看到那些修改被暂存到了, 哪些没有, 哪些文件没有被 Git tracked 到。git status 不显示已经 commit 到项目历史中去的信息。看项目历史的信息要使用 git log.

### 4.本地其他分支合并上传到远程主分支

以下操作是开发环境在本地分支开始的 1.代码上传到工作区： `git add .` 2.填写更新信息：`git commit -m '发布新建与编辑功能'` 3.切换到主分支：`git checkout master` 4.合并分支到主分支：`git merge dev` 5.拉取代码：`git pull` 6.上传代码: `git push`

### 5.本地分支上传到远程分支

如果想把本地的某个分支 test 提交到远程仓库，并作为远程仓库的 master 分支，或者作为另外一个名叫 test 的分支，那么可以这么做。

```
git push origin test:master         // 提交本地test分支作为远程的master分支 //好像只写这一句，远程的github就会自动创建一个test分支
git push origin test:test              // 提交本地test分支作为远程的test分支
```

如果想删除远程的分支呢？类似于上面，如果:左边的分支为空，那么将删除:右边的远程的分支。

```
git push origin :test              // 刚提交到远程的test将被删除，但是本地还会保存的，不用担心
```
