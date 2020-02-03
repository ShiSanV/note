# git在本地分为 工作区， 暂存区， 本地库

工作区为写代码的地方
打算提交到本地库的数据，临时存储
 存储历史版本及操作
从工作区--> 暂存区 命令为 git add
从暂存区--> 本地库 命令为 git commit

# 本地库初始化

git init

# 设置签名
## 形式

​    用户名：yxw
​    Email地址：yxw@aisino.com 

## 作用

​    区分不同开发人员的身份 Email地址 可以是无效的

##  辨析

​    签名和登录远程仓库（GitHub）的账号、密码没有任何关系

## 级别

### 项目级别/仓库级别：尽在当前本地库范围内有效

​        命令： git config user.name yxw
​              git config user.email yxw@aisino.com       

### 系统用户级别：登录当前操作系统的用户范围

​        命令：git config --global user.name yxw
​              git config --global user.email yxw@aisino.com

###  优先级

​            就近原则：项目级别优先于系统用户级别，二至都有事采用项目级别的签名

### 用户保存位置

​        项目级别：当前仓库目录下.git/config
​        系统级别： cat ~/.gitconfig

# 基本命令

git status 查看本地库的状态
git add xx 将xx文件提交到暂存区
git rm --cached xx 将提交到暂存区的xx文件撤回
git commit xx 将暂存区的xx文件提交到本地库 然后i后 输入提交的备注信息 
或者 git commit -m "提交的备注信息" xx 将暂存区的xx文件提交到本地库 可跳过输入备注信息
git log 查看操作的日志
    git log --pretty=oneline 查看简略操作日志
    git log --oneline 查看更加简洁的信息
    git reflog 可展示前进或回退步数

# 回退或者前进

首先使用命令行 git reflog 查看操作日志及其索引值
git reset --hard [索引值]
git rest 参数详解
    --soft 移动本地库指针
    --mixed 移动本地库指针 重置暂存区
    --hard 移动本地库指针 重置暂存区 重置工作区

# 删除本地库文件及找回

rm -rf xxx
git add xxx 
git commit -m "提交信息" xxx
说明： git本地库是不会删除文件的 所以如果想要找回删除的文件 只需要回退到未删除的版本即可
小技巧：如果不小心删除了工作区的文件 或者 删除后提交到了暂存区 但是并没有提交到本地库  可以实用
git reset --hard HEAD 来重置 暂存区和工作区

# 比较文件差异

git diff xxx
说明： 将工作区的指定文件和暂存区进行比较
git diff HEAD xxx
说明： 将工作区的指定文件和本地库进行比较
git diff
说明： 将工作区的所有文件和暂存区进行比较 
git diff HEAD
说明：将工作区的所有文件和本地库进行比较

# 分支
## 创建分支

​    git branch [分支名]

## 查看所有分支

​    git branch -v

## 切换分支

​    git checkout [分支名]

## 合并分支

​    切换到被合并的分支上

​            git checkout [被合并分支名]

​	 执行merge命令

​            git merge [要合并过来的分支名]

## 解决冲突

​    冲突原因：不同分支修改了同一文件的同一行 造成git无法自动合并
​    解决：执行 git merge [文件名] 命令后 手动修改合并后的文件， 
​    然后执行 git add [文件名]，
​    最后执行 git commit -m "日志信息" 注意 此处不要加[文件名]

# github远程仓库   

账号：375437043@qq.com 2326678432@qq.com 2758228527@qq.com
仓库地址：https://github.com/ShiSanV/aisino.git

## 提交远程仓库
### 添加远程仓库

​        git remote add [remote-name] [url]
​        

```shell
git remote add origin https://github.com/ShiSanV/aisino.git
```



###     查看远程仓库

​        git remote
​        说明：它会列出你指定的每一个远程服务器的简写。如果你已经克隆了自己的仓库，那么至少应该能看到 origin ——这是 Git 给你克隆的仓库服务器的默认名字

```shell
$ git remote

origin
```

​		 git remote -v | --verbose 列出详细信息，在每一个名字后面列出其远程url  此时， -v 选项(译注:此为 –verbose 的简写,取首字母),显示对应的克隆地址

```shell
$ git remote -v

origin  https://github.com/ShiSanV/aisino.git (fetch)
origin  https://github.com/ShiSanV/aisino.git (push)
```

### 推送到远程仓库

​	git push [remote-name] [branch-name]

```shell
$ git push origin master
```

### 获取远程仓库代码并和本地库进行合并

我们可以使用 git fetch  [remote-name] [branch-name]

```shell
$ git fetch origin master
```

将远程仓库代码下载到本地[remote-name]/[remote-branch-name]分支上

```shell
$ git fetch origin master

remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 326 bytes | 27.00 KiB/s, done.
From https://github.com/ShiSanV/aisino
 * branch            master     -> FETCH_HEAD
   2012128..bb74948  master     -> origin/master
```

说明：fetch 并不会将远程仓库代码更新当前工作环境 而是更新到了 origin/master分支上

切换到 origin/master分支

```shell
$ git checkout origin/master
```

查看分支信息

```shell
$ git branch -v

* (HEAD detached at origin/master) bb74948 aisino1 18点55分添加了部分信息
  master                           2012128 aisino1 modirfy aisino.txt
```

查看origin/master分支上的文件，已是最新的信息，若无任何问题 切换到主分支后 执行merge操作

```shell
$ git merge origin/master

Updating 2012128..bb74948
Fast-forward
 aisino.txt | 2 ++
 1 file changed, 2 insertions(+)
```

或者直接执行合并操作 **pull = fetch + merge**

```shell
$ git pull origin master

remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), 307 bytes | 21.00 KiB/s, done.
From https://github.com/ShiSanV/aisino
* branch            master     -> FETCH_HEAD
bb74948..f3fdd5f  master     -> origin/master
Updating bb74948..f3fdd5f
Fast-forward
aisino.txt | 2 ++
1 file changed, 2 insertions(+)

```

### 解决远程仓库冲突

注意：我们修改代码必须要在远程仓库最后一次提交的基础上修改， 如果在我们从远程仓库拉去代码后，有别人去提交了代码， 那么我们在提交的时候就会产生冲突， 原因是因为文件的hash值不一样（参考git的操作原理）

解决思路：

首先获取远程仓库最新版代码

```shell
$ git pull origin master

remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), 281 bytes | 25.00 KiB/s, done.
From https://github.com/ShiSanV/aisino
 * branch            master     -> FETCH_HEAD
   f3fdd5f..7da75e1  master     -> origin/master
Auto-merging aisino.txt
CONFLICT (content): Merge conflict in aisino.txt
Automatic merge failed; fix conflicts and then commit he result.

```

由以上最后三行信息可知 git在帮我们自动合并远程仓库代码和本地代码时 发生了冲突 这里需要我们参考

分支中出现冲突的解决办法<a href="#解决冲突">点击这里</a>

## 远程分支工作流程

在工作环境中 如果有新的模块需要开发 则应该创建一个新的分支 一定不要在master分支上操作，开发完成后 提交到 新的分支， 等待开发经理核验后 在他的本地进行代码的合并 然后再推送到远程仓库





​     

​			