# Git 使用

## 基本操作流程

### 独立新分支开发

- 初始化 git       git  init
- 跟远程仓库建立联系   git remote  add origin   仓库地址
- 切换到和远程对应分支   git  checkout   分支
- 拉取远程仓库代码 ，本地和远程保持唯一   git pull  --rebase  origin  分支
- 编写代码
- 添加本地文件到暂存区

	- 添加文件   

		- 添加所有文件

			- git  add  .

		- 添加文件夹

			- git  add 文件夹名/

		- 添加文件

			- git   add 文件      必须有后缀名

	- 添加注释

		- git  commit -m  '注释信息'

- 将暂存区的内容提交到远程仓库

	- git  push  -u  oirgin  分支

### 以上操作 对于平常开发够解决了，更加深入可以看下面操作

## 常用指令

### 分支操作

- 查看所有分支

	- git  branch -a

- 合并分支：git merge 原分支  目标分支
- 查看远程分支

	- git branch -a或git branch -r

- 创建本地分支     git branch   分支名
- 切换本地分支    git  checkout   分支名
- 删除本地分支    git branch -d demo
- 删除远程分支   git  push origin  : 分支名字      或者   git push origin --delete  分支名
- 创建远程分支，将本地分支代码 提交到远程分支   

	- git push origin demo_fenzhi:demo_fenzhi    本地分支名称:远程分支名称

- 合并某分支到当前分支

	- git merge  分支名

- 本地分支关联远程分支

	- git  pull origin   分支名

### 回到历史版本

- 本地已经 push  到远程仓库处理情况     git  revert

	- 查看回退版本号

		- git  log

	- 回退本地仓库

		- git  revert   回退版本的hash

	- 添加本地文件

		- git  add   .

	- 提交到远程仓库

		- git push origin  分支名

- 本地没有push，  git  commit  出现问题，想回退版本  git  reset

	- 查看回退版本号

		- git  log

	- 回退本地仓库

		- git  reset --hard  版本hash

	- 想要回到未来的版本

		- git  reflog  获取未来版本号

### 删除放入暂存区的内容

- git  rm  文件名

	- 将该文件从commit后撤回到add后

- git reset HEAD^ --hard 

	- 删除后 可以用git rm 文件名再回撤一步

### 查看信息

- git status

	- 查看当前提交的状态

- git   log

	- 查看历史提交

- git  branch

	- 查看本地所有分支

- git   remote -v

	- 查看远程版本信息

