
# git 安装

## Win 下载
https://git-scm.com/download/
## Linux
```bash
apt-get install git
```

# git 配置
	注意： win 在 Git Bash 下； linux 在 /bin/bash 即可

##### 生成公钥
```bash
ssh-keygen -t rsa -C "your_email@xxx.com"
```

##### 获取公钥
```bash
# linux:
cat ~/.ssh/id_rsa.pub
#win: 
C:/user/.ssh/id_rsa.pub
```

##### 配置公钥
复制公钥到 gitee｜githbu 网页个人设置中的公钥之中；保存。

##### 终端环境配置
```bash
ssh -T git@gitee.com

git config --global user.name yourname  # "你码云的名字或昵称"

git config --global user.email youremail@xxx.com # "你码云的主邮箱"

```

##### vscode 用一个远程库
**初始化一个本次仓库**
ctrl+shift+p initialize repository: 
**远程链接**
源代码管理库-》远程-》远程仓库-》粘贴.git 链接-》ok


# git 使用
##### 远程仓库管理

git remote -v:列出所有远程仓库的别名和地址

git remote show <远程仓库别名>:查看远程仓库的详细信息

git remote add <别名> <仓库地址>:添加一个新的远程仓库

git remote remove <别名>:删除一个远程仓库

git remote rename <原别名> <新别名>:重命名一个远程仓库

git remote set-url <别名> <新地址>:修改某个远程仓库的地址

git remote set-head <别名> <分支名>:修改远程仓库的默认分支

git remote -v update:更新所有远程仓库

git remote prune <别名>:移除远程上已删除的tracking分支

git remote get-url <别名>:查看远程仓库的原始克隆URL



#### 查看跟踪远程分支

git branch -vv (当前分支：hash：远程跟踪分支：注释)

#### 修改跟踪的远程分支

git branch --set-upstream-to=origin/remote-branch local-branch

这将为本地分支 `local-branch` 设置远程跟踪分支为 `origin/remote-branch`。

你也可以使用简化命令来设置远程跟踪分支：

git branch -u origin/remote-branch local-branch

（这个命令用于现有本地分支，将其与远程分支相关联）

#### 创建新分支

git branch  origin/serverfix



#### 切换本地分支  

git checkout master 

  这里的master应该就是本地的分支



此外：

git checkout -b master origin/master 

  表示切换的分支是新建的（本地只有main，没有master），分支名：master； 来源：orgin/master;


确定要删除的分支，并运行以下命令来删除该分支（假设要删除的分支名为 branch-name）：

git branch -d branch-name

如果分支上有未合并的更改，Git 会拒绝删除该分支，并显示一条警告消息。如果确实要强制删除分支，可以使用 -D 选项：

git branch -D branch-name



#### 拉取

源代码管理-》拉取

或者 pull 就行

git pull [远程仓库名] [远程分支]




#### 提交

手动

添加消息-》提交-》同步

命令提交

暂存：git add * 


## 提交：

使用与当前分支关联的远程仓库和同名的远程分支：

git push 
git push  xinka master

git push [<远程仓库名>] [<本地分支名>:]<远程分支名>

强制将当前分支 覆盖掉origin 的 master 分支：
git push origin +current-branch:master




（下面几个命令可以用来解决：refuse to merge unrelated histories）

#### 获取分支最新提交hash

git log -1 main


从远程仓库获取最新的提交和分支信息，但不会自动将这些更改合并到你的本地分支中：

git fetch <远程仓库名>

使用其他命令（如git merge或git rebase）将这些更改合并到你的本地分支中。

## git merge 

git merge --strategy=ours <branch-to-merge>
```
这将执行合并操作，但将忽略其他分支（`<branch-to-merge>`）的更改，仅保留当前分支（主分支）的内容。任何冲突都会被自动解决为当前分支的更改。

git merge --strategy=theirs <branch-to-merge>
```
这将执行合并操作，但会忽略当前分支的更改，选择其他分支（`<branch-to-merge>`）的内容。冲突会被自动解决为其他分支的更改。


# 放弃本地分支，重置到master分支的最新提交

1. git fetch

2. git reset --hard <dev-latest-commit-hash>

或者

2. git reset --hard origin/master



另一种解释：放弃本地更改：

git reset --hard FETCH_HEAD，其中FETCH_HEAD表示上一次成功pull之后的commit点，然后git pull即可


# 强制推送当前分支到远程分支

git push -f xinka master

这样就丢掉了 master 原来的历史。这样可以快速建立分支之间的关系，但是原有分支的历史提交会丢失。需要根据实际情况权衡是否使用这种破坏性的重置方式。
上面可能会出现：error: src refspec localagent does not match any ，此时，试试下面的操作。

git fetch
git push -f orign  HEAD:<远程分支名>



# 补充

git pull 后 仓库  在本地的默认名称  就是  origin

查看：git remote -v 

修改：git remote rename old new

比如： git push origin master 表示将本地 推到 master

永久修改默认名称：git config --global init.defaultBranch new_name;


#  强制覆盖本地分支

    git fetch --all

    git reset --hard origin/master

    git pull



## 强制覆盖远程分支

git push [<远程仓库名>] [<本地分支名>:]<远程分支名>

每次提交之后，必须点击：同步更改才能看到网页的变化。

# 问题解决
## connect reset 问题，直接通过命令行配置解决
git config --global http.sslBackend "openssl"

git config --global http.sslCAInfo "C:\Program Files\Git\mingw64\ssl\cert.pem"




## git remote

git remote [-v | --verbose]

显示所有远程仓库的名称，以及它们的 URL。
示例：git remote -v
git remote add [-t <branch>] [-m <master>] [-f] [--tags | --no-tags] [--mirror=<fetch|push>] <name> <url>

添加一个新的远程仓库。
示例：git remote add origin https://github.com/user/repo.git
git remote rename [--[no-]progress] <old> <new>

重命名一个远程仓库。
示例：git remote rename origin upstream
git remote remove <name>

移除指定名称的远程仓库。
示例：git remote remove origin
git remote set-head <name> (-a | --auto | -d | --delete | <branch>)

设置指定远程仓库的 HEAD 引用。
示例：git remote set-head origin master
git remote show [-n] <name>

显示指定远程仓库的信息，包括分支和跟踪情况。
示例：git remote show origin
git remote prune [-n | --dry-run] <name>

移除没有对应本地分支的指定远程仓库分支的引用。
示例：git remote prune origin
git remote update [-p | --prune] [(<group> | <remote>)...]

更新指定的远程仓库的引用。
示例：git remote update origin
git remote set-branches [--add] <name> <branch>...

设置指定远程仓库应该跟踪的分支。
示例：git remote set-branches origin main
git remote get-url [--push] [--all] <name>

获取指定远程仓库的 URL。
示例：git remote get-url origin
git remote set-url [--push] <name> <newurl> [<oldurl>]

设置指定远程仓库的 URL。
示例：git remote set-url origin https://github.com/user/new-repo.git
git remote set-url --add <name> <newurl>

添加一个新的 URL 到指定远程仓库。
示例：git remote set-url --add origin https://github.com/user/old-repo.git
git remote set-url --delete <name> <url>

从指定远程仓库中删除指定的 URL。
示例：git remote set-url --delete origin https://github.com/user/old-repo.git


# 创建一个新的分支来保留你所做的提交

git switch -c <new-branch-name>  :这将在当前提交处创建一个新的分支，并切换到该分支，让你可以在该分支上继续工作

如果放弃处于分离头指针状态下的更改，执行：git switch -  ：将切换回进入分离头指针状态之前所在的分支


# 将一个本地仓库 提交到另一个本地仓库跟踪的远程分支

确保你当前在本地分支 pv 上。可以使用以下命令检查当前所在的分支：


git branch
确保 pv 分支前面有 * 标记。

首先，将本地分支 pv 推送到远程的 private/master 分支。执行以下命令：

git push private pv:master
这将把本地分支 pv 推送到远程仓库的 private/master 分支。

接下来，切换到本地分支 pv 并合并远程的 public/master 分支。执行以下命令：

git checkout pv
git merge public/master
这将切换到分支 pv 并将远程的 public/master 分支合并到本地分支。

最后，将合并后的更改推送到远程的 public/master 分支。执行以下命令：


git push public pv:master
这将把本地分支 pv 推送到远程仓库的 public/master 分支。


