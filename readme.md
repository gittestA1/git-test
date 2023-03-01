# =====Git=====
Git介绍 分布式版本控制工具 VS 集中式版本控制工具
Git安装 基于官网发布的最新版本2.31.1 安装讲解
Git命令 基于开发案例 详细讲解了git的常用命令
Git分支 分支特性 分支创建 分支转换 分支合并 代码合并冲突解决
Idea 集成Git
# =====GitHub=====
创建远程库
代码推送 Push
代码拉取 Pull
代码克隆 Clone
SSH免密登录
Idea 集成GitHub
# =====Gitee码云=====
码云创建远程库
Idea 集成Gitee码云
码云连接GitHub 进行代码的赋值和迁移
# =====GitLab=====
GitLab服务器的搭建和部署
Idea 集成GitLab

课程目标：五小时熟练掌握Git GitHub GitLab Gitee码云的使用
# 1 Git常用命令
## 1-1 设置用户签名(options)
```
git config --global user.name xxx
git config --global user.email xxx@xxx.xxx
```

## 1-2 初始化本地库
```
git init
```
查看 ./git
```
ll 或 ll -a（查看隐藏文件）
```
## 1-3 查看本地库状态
```
git status
```
### 1-3-1 首次查看（工作区没有任何文件）
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
### 1-3-2 新增文件（hello.txt）
```
vim hello.txt
```
进入insert模式
hello.world!hello git!
退出insert模式,将光标放至第一行
yy 复制
p 粘贴
dd 删除 
:wq保存并退出

查看hello.txt
```
cat hello.txt
```
查看hello.txt最后一行
```
tail -n 1 hello.txt
```
### 1-3-3 再次查看（检测到未追踪的文件）
```
git status
```
On branch master
No commits yet
Untracked files:
    (use "git add<file>..." to include in what will be committed)
        hello.txt
nothing added to commit but untracked files present (use "git add" to track)

## 1-4 将工作区文件添加至暂存区
```
git add hello.txt
```
### 1-4-1 查看状态（检测到暂存区有新文件）
```
git status
```
On branch master
No commits yet
Changes to be committed:
    (use "git rm --cached<file>..." to unstage)
        new file: hello.txt
### 1-4-2 移出暂存区
```
git rm --cached hello.txt
&&
git restore --staged hello.txt 
```
### 1-4-3 移出工作区
```
git restore hello.txt
```

## 1-5 将暂存区文件提交到本地库
```
git commit -m "first commit" hello.txt
```
[master (root-commit) 6041030] first commit
    1 file changed, 17 insertions(+)
    create mode 100644 hello.txt
* 提交修正commit --amend
提交修正就是对上一次的提交重写，在log中，提交修正的信息会覆盖之前提交的信息
```
git commit --amend -m 'info'
```
1.提交的代码有问题；或者漏提交了一些文件；或者提交信息写错了；
2.当项目复杂，开发周期较长，针对特定功能开发时，会产生大量的commit记录，导致日志冗长且重点模糊。

### 1-5-1 查看版本信息（精简版）
```
git reflog
```
6041030 (HEAD -> master) HEAD@{0}: commit (initial): first commit

6041030为完整版本号的前七位的精简版本号

* reflog引用日志：
记录了HEAD节点和分支引用所指向的历史
简明地说：reflog记录所有的更改，是当你的项目损坏时（只要提交过），它给了你挽回的机会。
而且它只保留在本地库（不能push），且默认只保留90天
引用日志中：head改变+commit的提交都会被记录
### 1-5-2 查看版本信息（完整版）
```
git log
```
commit 604103097285f531a727419a3d6377addfa7d5a9 (HEAD -> master)
Author: dong <dong@gmail.com>
Date:   Wed Feb 15 12:01:01 2023 +0800
    first commit

普通log是版本库的一部分，可以随版本库的push而迁移
##### 一、显示
1.限制输出条数
显示最新的n条日志
```
git log -n
```
2.每行一个日志，显示关键信息
```
git log --oneline
```
3.日志的图表显示
```
git log --graph
```
4.对每次提交文件进行统计
```
git log -stat
```
5.输出文件修改的内容
```
git log --patch
或
git log -p
```
#### 二、筛选
1.按日期进行筛选
```
git log --after='YY-mm-DD' after不包含日期
git log --before='YY-mm-DD' before包含日期
git log --after='YY-mm-DD' --before='YY-mm-DD' (after, before]
git log --before='today' 等同与 git log
git log --before='yesterday' 查看今天之前
git log --before='30 day ago'
git log --after='4 week ago'
git log --after='1 month ago'
```
2.按作者筛选
```
git log --author='username or mail'
```
注意：这个用户名不用完全匹配
譬如：--author='小明'，则王小明、李小明都能被匹配,邮箱同理
3.按提交信息筛选
```
git log --grep='info'
```
注意：这个提交信息不用完全匹配
4.选项组合
```
git log --grep='info' --oneline -1
```
筛选提交信息为info，每条记录显示一行，且只显示最新的一条提交
可以任意组合
### 1-5-3 查看状态（没有文件需要提交）
```
git status
```
On branch master
nothing to commit, working tree clean

## 1-6 修改文件（hello.txt）
```
vim hello.txt
```
### 1-6-1 查看状态（监测到工作区有文件被修改）
```
git status
```
On branch master
Changes not staged for commit:
    (use "git add <file>..." to update what will be commited)
    (use "git restore <file>..." to discard changes in working directory)
        modified: hello.txt
mp changes added to commit (use "git add" and/or "git commit -a")
### 1-6-2 检修改的文件再次添加暂存区
```
git add hello.txt
```
### 1-6-3 查看状态（工作区的修改添加到了暂存区）
```
git status
```
On branch master
Changes not staged for commit:
    (use "git restore --staged <file>..." to unstaged)
        modified: hello.txt
### 1-6-4 将暂存区文件提交到本地库
```
git commit -m "second commit" hello.txt
```
[master 939c552] second commit
    1 file changed, 1 insertions(+), 1 deletion(-)
    修改一行————底层处理为一行删除，一行新增
### 1-6-5 查看状态（没有文件需要提交）
```
git status
```
On branch master
nothing to commit, working tree clean
### 1-6-6 查看版本信息
```
git reflog
```
939c552 (HEAD -> master) HEAD@{0}: commit: second commit
6041030 HEAD@{1}: commit (initial): first commit
底层是通过指针来控制版本信息的

## 1-7 历史版本
### 1-7-1 查看历史版本
```
git reflog
```
### 1-7-2 版本穿梭
```
git reset --hard 6041030
```
HEAD is now at 6041030 first commit
6041030为精简版本号
```
git reflog
```
6041030 (HEAD -> master) HEAD@{0}: reset: moving to 6041030
11ccb9f HEAD@{1}: commit: third commit
939c552 HEAD@{2}: commit: second commit
6041030 (HEAD -> master) HEAD@{3}: commit (initial): first commit
```
git reset --hard 11ccb9f
```
HEAD is now at 11ccb9f third commit
```
git reflog
```
11ccb9f (HEAD -> master) HEAD@{0}: reset: moving to 11ccb9f
6041030 HEAD@{1}: reset: moving to 6041030
11ccb9f (HEAD -> master) HEAD@{2}: commit: third commit
939c552 HEAD@{3}: commit: second commit
6041030 HEAD@{4}: commit (initial): first commit

git切换版本，底层其实是移动HEAD指针
head-->master-->first
head-->master-->second
head-->master-->third
head-->master-->???

# 2 Git分支操作
## 2-1 什么是分支
在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是一个单独的副本（分支底层其实也是指针的运用）
hot-fix                 1.1-->fix-->1.2
master          1.0 --> 1.1 --> 1.2 --> 2.0
feature-blue    1.0-->blue-->1.1
feature-game    1.0 --> 1.1.1 --> 1.1.2 --> game --> 2.0

主分支（主干/主线/main/master）
包含所有最终修改的历史，反映项目的最终版本
建议不要乱动主干。如果你编辑了一个小组项目的主干分支，你的改动会影响到其他人，而且很快就会出现合并冲突

开发分支（集成分支/develop/dev）
与主分支平行。该分支包含了为下一个版本所做的最新开发修改。它拥有该版本的最终源代码
当开发分支到达稳定状态并准备发布时，应与主干分支合并，并标记为发布版本
## 2-2 分支的好处
同时并行推进多个功能开发，提高开发效率
各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可
## 2-3 分支的操作
git branch 分支名 ————创建分支 
git branch -v ————查看分支
git checkout 分支名 ————切换分支
git merge 分支名 ————把指定的分支合并到当前分支上
### 2-3-1 查看分支
```
git branch
```
    * master
```
git branch -v
```
    * master 11ccb9f third commit
```
git branch -a
```
    hot-fix
    * master
    remotes/origin/hot-fix
    remotes/origin/master
查看所有分支，本地+远程
### 2-3-2 创建分支
```
git branch hot-fix
```
```
git branch -v
```
      hot-fix 11ccb9f third commit
    * master  11ccb9f third commit
创建版本库同时创建分支
-b branch
```
git init -b dev
```
分支重命名
```
git branch -m <旧分支名称> <新分支名称>
```
分支删除
```
git branch -d <分支名称>
```
-d 为 delete，删除
-d 用于删除已合并过的分支
-D 强制删除分支（不管是否合并过）

创建并切换为分支
```
git checkout -b <新的分支名称>
```
### 2-3-3 切换分支
* 从版本库中恢复文件
* 1.已删除（或修改），未add
```
git checkout <文件路径>
```
* 2.已删除（或修改），已add，未commit
```
git checkout commitedId <文件路径>
```
commitId从git reflog中查看commit id
* 3.已删除（或修改），已commit
```
git checkout commitId <文件路径>
```
```
git checkout hot-fix
```
switched to branch 'hot-fix'
```
git branch -v
```
    *  hot-fix 11ccb9f third commit
       master  11ccb9f third commit
```
git status
```
On branch hot-fix
nothing to commit, working tree clean
```
vim hello.txt
git add hello.txt
git commit -m "hot-fix first commit" hello.txt
```
[hot-fix 24a41be] hot-fix first commit
 1 file changed, 2 insertions(+), 2 deletions(-)
```
git reflog
```
24a41be (HEAD -> hot-fix) HEAD@{0}: commit: hot-fix first commit
11ccb9f (master) HEAD@{1}: checkout: moving from master to hot-fix
11ccb9f (master) HEAD@{2}: reset: moving to 11ccb9f
6041030 HEAD@{3}: reset: moving to 6041030
11ccb9f (master) HEAD@{4}: commit: third commit
939c552 HEAD@{5}: commit: second commit
6041030 HEAD@{6}: commit (initial): first commit

## 2-3-4 合并分支（正常合并）
```
git checkout master
```
switched to branch master
合并至master分支
```
git merge hot-fix
```
updating 11ccb9f..24a41be
Fast-forward
 hello.txt | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
```
cat hello.txt
```
master分支与hot-fix分支一致

### 2-3-4 合并分支（冲突合并）
冲突产生的原因：
合并分支时，两个分支在同一个文件同一个位置有两套完全不同的修改。Git无法替我们决定使用哪一个，必须人为决定新代码内容
```
git checkout hot-fix
vim hello.txt
git add hello.txt
git commit -m "hot-fix test" hello.txt
git checkout master
vim hello.txt
git add hello.txt
git commit -m "master test" hello.txt
```
```
git merge hot-fix
```
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed: fix conflicts and then commit the result
```
git status
```
On branch master
You have unmerged paths.
    (fix conflicts and run "git commit")
    (use "git merge --abort" to abort the merge)
Unmerged paths:
    (use "git add <file>..." to mark resolution)
        both modified: hello.txt
no changes added to commit (use "git add" and/or "git commit -a")

    <<<<<<< HEAD
    hello world! hello git! master test
    hello world! hello git!
    =======
    hello world! hello git!
    hello world! hello git! hot-fix test
    >>>>>>> hot-fix
通过<<<<<<< HEAD、=======、>>>>>>> hot-fix这三个标识来分割代码
上半块为当前分支代码，下半块为合并分支代码
通过手动取舍冲突代码，完成后将这三个标识删除

    hello world! hello git! master test
    hello world! hello git! hot-fix test
```
git add hello.txt
```
```
git commit -m "merge test" hello.txt
```
fatal:cannot do a partial commit during a merge.
此时使用git commit不能带文件名
```
git commit -m "merge test"
```
[master 1a3e518] merge test
```
cat hello.txt
```
代码为手动合并后的结果
```
git checkout hot-fix
cat hello.txt
```
hot-fix中的hello.txt不受合并的影响

master、hot-fix其实都是指向具体版本记录的指针。当前所在的分支，其实是由HEAD决定的。所以创建分支的本质就是多创建一个指针。
HEAD如果指向master，那么我们现在就在master分支上
HEAD如果指向hot-fix，那么我们现在就在hot-fix分支上
所以切换分支的本质就是移动HEAD指针

# 2 Git团队协作机制
## 2-1 团队内协作
A与B为统一团队
A创建自己的本地库并使用push将本地库推送至代码托管中心（GitHub、码云...）,即远程库，
可以从远程库中clone A push的本地库，作为自己的本地库，修改后便可以push至远程库（前提：B获得A给予的权限，即A将B添加至自己的团队中），A便可以从远程库中pull拉取B push的本地库来更新自己的本地库，此时A本地库、B本地库、远程库达成同步
## 2-2 跨团队协作
A与B为不同团队
A创建本地库并push至远程库，B通过fork（叉子）将A的远程库整体复制一份至自己的远程库，使用clone（克隆）至B本地库，修改完成后push至自己的远程库，B需要发一个Pull request（拉取请求）至A，经A审核后便可将B的远程库merge至自己的远程库，使用pull将远程库拉取至本地库，A团队便可以修验该库



# 3 远程仓库操作
## 3-1 创建远程仓库别名
```
GitHub Token
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
```

* 查看远程库别名
```
git remote -v
```
* 添加别名git，地址为GitHub上的远程库链接
```
git remote add git https://github.com/gittestA1/git.git
git remote -v
```
git	https://github.com/gittestA1/git.git (fetch)
git	https://github.com/gittestA1/git.git (push)
拉取fetch和推送push的别名都为git

2021.8.13起，Github要求使用基于令牌的身份验证
```
git remote set-url git  https://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@github.com/gittestA1/git.git
```
git	https://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@github.com/gittestA1/git.git (fetch)
git	https://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@github.com/gittestA1/git.git (push)
```
git push git master
```
枚举对象中: 21, 完成.
对象计数中: 100% (21/21), 完成.
使用 4 个线程进行压缩
压缩对象中: 100% (14/14), 完成.
写入对象中: 100% (21/21), 1.57 KiB | 400.00 KiB/s, 完成.
总共 21（差异 6），复用 0（差异 0），包复用 0
remote: Resolving deltas: 100% (6/6), done.
To https://github.com/gittestA1/git.git
    * [new branch]      master -> master

推送master分支本地库至远程库

* 关联远程库
```
git remote add <远程库名称> <远程库地址>
```
* 推送远程库
```
git push -u <远程库名称> <推送分支名称>
```
在git中，“push -u”的意思是将本地的分支版本上传到远程合并，并且记录push到远程分支的默认值；当添加“-u”参数时，表示下次继续push的这个远端分支的时候推送命令就可以简写成“git push”

* 推送所有分支至远程库
```
git push <远程库名称> --all
```

* 删除远程库分支
```
git push <远程库名称> -d <远程分支名称>
```

* 远程仓库名称的更改
```
git remote rename <旧名称> <新名称>
```
* 移除已关联的远程库
```
git remote remove <远程库名称>
```
## 3-2 拉取远程库
```
git pull git master
```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
展开对象中: 100% (3/3), 641 字节 | 320.00 KiB/s, 完成.
来自 https://github.com/gittestA1/git
    * branch            master     -> FETCH_HEAD
   1a3e518..50b2242  master     -> git/master
更新 1a3e518..50b2242
Fast-forward
 hello.txt | 1 +
 1 file changed, 1 insertion(+)
## 3-3 克隆远程库
创建一个新文件夹git_A
```
git clone https://github.com/gittestA1/git
```
正克隆到 'git'...
remote: Enumerating objects: 24, done.
remote: Counting objects: 100% (24/24), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 24 (delta 7), reused 20 (delta 6), pack-reused 0
接收对象中: 100% (24/24), 完成.
处理 delta 中: 100% (7/7), 完成.
```
cd git/
git remote -v
```
origin	https://github.com/gittestA1/git.git (fetch)
origin	https://github.com/gittestA1/git.git (push)

也可使用 https://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@github.com/gittestA1/git.git 来进行克隆

clone会做如下操作：
1.拉取代码
2.初始化本地仓库
3.创建别名

## 3-4 团队内协作

```
vim hello.txt
git add hello.txt
git commit -m "git_1 first commit" hello.txt
git push https://github.com/gittestA1/git.git/ master
```
Username for 'https://github.com': githubak47
Password for 'https://githubak47@github.com': 
remote: Permission to gittestA1/git.git denied to githubak47.
致命错误：无法访问 'https://github.com/gittestA1/git.git/'：The requested URL returned error: 403

密码为账号token
A未将B的github账号拉入团队，在B的github中打开A发送的邀请链接便可加入团队

```
git push https://github.com/gittestA1/git.git/ master
```
Username for 'https://github.com': githubak47
Password for 'https://githubak47@github.com': 
枚举对象中: 5, 完成.
对象计数中: 100% (5/5), 完成.
使用 4 个线程进行压缩
压缩对象中: 100% (2/2), 完成.
写入对象中: 100% (3/3), 279 字节 | 279.00 KiB/s, 完成.
总共 3（差异 1），复用 0（差异 0），包复用 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/gittestA1/git.git
   f5ad821..c8cbd31  master -> master

回到A的git下
```
git pull git master
```
remote: Enumerating objects: 14, done.
remote: Counting objects: 100% (14/14), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 12 (delta 4), reused 12 (delta 4), pack-reused 0
展开对象中: 100% (12/12), 941 字节 | 44.00 KiB/s, 完成.
来自 https://github.com/gittestA1/git
    * branch            master     -> FETCH_HEAD
   50b2242..c8cbd31  master     -> git/master
更新 50b2242..c8cbd31
Fast-forward
 hello.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
```
cat hello.txt
```
与远程库及B的本地库一致

## 3-5 跨团队协作
C fork A 
C 修改
C pull requests
A 审核后 merge pull request
A 远程库与 C 同步

## 3-6 SSH免密登录
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
```
cd ~/.ssh
cat id_ras.pub
```
将公钥复制粘贴至github中
```
git push git@github.com:gittestA1/git.git master
```
无需登录便可进行远程库操作

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
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