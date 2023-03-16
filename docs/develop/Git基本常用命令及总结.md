# Git基本常用命令及总结

## 一、创建版本库

```shell
# 初始化版本库（随后文件夹会生成一个.git的文件夹，是git的版本库）
git init

# 初始化git信息
git config --global user.name '用户名'
git config --global user.email '邮箱地址'
```

## 二、工作区和暂存区

?> Git控制系统分为工作区和暂存区两个概念。工作区就是此次项目创建的本地文件夹，在项目中使用 `git` 命令创建版本库之后，工作区会生成一个 `.git` 的文件夹，里面存放了很多东西，其中最重要的就是称为 `stage` 的暂存区，还有Git为我们自动创建的第一个分支 `master`，以及指向 `master`的一个指针叫`HEAD` 。
>

>
> 我们把文件往Git版本库里添加的时候，是分两步执行的：
>
> ​	第一步：使用 `git add` 命令将文件修改添加到暂存区；
>
> ​	第二步：使用 `git commit` 命令将当前分支中暂存区的所有内容提交到版本库中。



![git-repo](images/git.jpeg)

## 三、添加和提交文件

```shell
# 将readme.txt文件添加到暂存区
git add readme.txt
# 将所有的文件添加到暂存区
git add . 

# 将暂存区的所有文件提交到本地版本库中
git commit -m '提交时的备注'
```

## 四、撤销修改

```shell
# 查看当前状态
git status

$ git status
On branch master
# 还未添加到暂存区时的状态
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt
        
$ git status
# 添加到暂存区后的状态（待提交）
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   readme.txt


# 撤销工作区的修改
# 比如：在readme.txt文件中，修改了某些内容，但还未将文件添加到暂存区中，若此时想放弃本次修改的内容，则使用以下命令即可放弃修改，readme.txt文件中的内容就会恢复到修改内容之前
git checkout -- readme.txt 
或 
git restore readme.txt （新版本Git，建议使用）

# 撤销暂存区的修改
# 比如：在readme.txt文件中，修改了某些内容，并且将文件添加到了暂存区中，若此时想放弃本次添加到暂存区中的文件，则使用以下命令即可放弃添加，暂存区会恢复到添加文件之前
git reset HEAD readme.txt
或 
git restore --staged readme.txt（新版本Git，建议使用）
```

## 五、删除文件

```shell
# 1.本地删除readme.txt文件
rm -f readme.txt

# 2.将删除的文件提交到暂存区
git add readme.txt
或
git rm readme.txt

# 3.提交到本地仓库
git commit -m '删除readme.txt文件'

# 若本地误删除某个文件，但未提交到本地版本库，则可以使用上节所述的撤销修改命令来恢复文件（删除文件本身也是一种修改）
git restore 文件名
git restore --staged 文件名
```

## 六、版本回退

```shell
# 查看提交的日志
git log 

$ git log
# 下面的字符串表示提交的版本号
commit 45655a0a6041eaf6190f6c279b81096f9bef04f9 (HEAD -> master) 
Author: 别回头丶 <312885991@qq.com>
Date:   Wed Jan 12 14:02:12 2022 +0800

    第二次提交

commit 121d79b208a791180b621cc14da33c32c6ef0244
Author: 别回头丶 <312885991@qq.com>
Date:   Wed Jan 12 13:15:11 2022 +0800

    第一次提交
    
# 以图表的方式展示日志
git log --graph

$ git log --graph
* commit 45655a0a6041eaf6190f6c279b81096f9bef04f9 (HEAD -> master) 
| Author: 别回头丶 <312885991@qq.com>
| Date:   Wed Jan 12 14:02:12 2022 +0800
|
|     第二次提交
|
* commit 121d79b208a791180b621cc14da33c32c6ef0244
| Author: 别回头丶 <312885991@qq.com>
| Date:   Wed Jan 12 13:15:11 2022 +0800
|
|     第一次提交

# 查看操作日志
git reflog

$ git reflog
# 首位的字符串表示版本号（版本号的前几位）
45655a0 (HEAD -> master) HEAD@{0}: commit: 第二次提交
121d79b HEAD@{1}: commit (initial): 第一次提交
  
 
# 版本回退
# 首先，要进行版本回退，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新一次的
# 提交45655a0...，而上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本写100个^比较容易写不
# 过来，所以写成HEAD~100。

git reset --hard 指定的版本(使用HEAD或者版本ID)

# 回退到上一个版本
git reset --hard HEAD^

# 回退到指定的版本
git reset --hard 45655a0
```

## 七、远程仓库

```shell
# 本地版本库关联远程版本库
git remote add origin https://gitee.com/LLS312885991/test.git

# 将本地仓库中的当前分支推送到远程仓库中所对应的分支
# 若多人协作时，有其他开发者先于自己push到了远程仓库，则自己先要pull拉取最新的提交，并且在本地合并之
# 后，才能push成功
git push
git push origin master
git push origin dev

# 由于远程仓库是空的，所以我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推# 送到远程新的master分支上，还会把本地的master分支和远程的master分支关联起来，这样在以后的推送或者拉
# 取时就可以简化命令，直接使用 git push 或者 git pull
git push -u origin master

# 拉取远程仓库的代码同步到本地仓库
git pull

# 查看关联的远程仓库信息
git remote -v

$ git remote -v
origin  https://gitee.com/LLS312885991/test.git (fetch)
origin  https://gitee.com/LLS312885991/test.git (push)

# 解除本地仓库与远程仓库的关联绑定
git remote rm origin

# 克隆远程仓库到本地
git clone https://gitee.com/LLS312885991/test.git
```

## 八、分支管理

> 分支的作用非常强大，它使得多人工作开发时，每个人都有自己独立的开发空间，互不干扰。一般多人开发时，每个人都会先创建属于自己的分支，然后进行开发，待开发完成后，再合并到主分支上，最后再删除自己的分支即可。

```shell
# 创建分支
git branch 分支名称
git branch dev

# 查看所有分支（*代表当前分支）
git branch

$ git branch
  dev
* master

# 切换到dev分支
git switch dev

$ git switch dev
Switched to branch 'dev'

# 创建并切换到dev分支
git switch -c dev

$ git switch -c dev
Switched to a new branch 'dev'

# 将dev分支上的内容合并到当前分支（此命令只能简单合并，即两个分支上所修改的内容无冲突）
git merge dev

# 删除dev分支
git branch -d dev

# 合并冲突（即两个分支上修改同一个文件，且修改的内容不一致时，会出现合并冲突，此时需要手动解决冲突）
$ git merge dev
Auto-merging a.txt
CONFLICT (content): Merge conflict in a.txt # 显示合并冲突，且告知是在a.txt文件中有冲突
Automatic merge failed; fix conflicts and then commit the result.

$ cat a.txt 
<<<<<<< HEAD # 显示的是当前分支上，a.txt中修改的内容
hello world! liulusheng
=======
hello world! dev # 显示的是dev分支上，a.txt中修改的内容
>>>>>>> dev

$ vim a.txt
hello world! liulusheng # 手动解决冲突，修改为需要保留的内容

$ git add a.txt # 再添加该文件到暂存区

$ git commit -m '合并冲突' # 再提交该文件到版本库即可
[master ceeba28] 合并冲突

# 使用git log也可以查看合并情况，--graph表示以图表的方式展示
git log --graph

$ git log --graph
*   commit ceeba28f86befa755d72d61155e3a4bb95d04bb3 (HEAD -> master, origin/master)
|\  Merge: f74d466 6aaee45
| | Author: 别回头丶 <312885991@qq.com>
| | Date:   Wed Jan 12 18:29:17 2022 +0800
| |
| |     合并冲突
| |
| * commit 6aaee45e1e0838a0f8b9a1d0ab368dddf030e41e (dev)
| | Author: 别回头丶 <312885991@qq.com>
| | Date:   Wed Jan 12 18:22:19 2022 +0800
| |
| |     dev分支上修改a.txt
| |
* | commit f74d4662153a4a665e638352df528c3c9cb3bdbd
|/  Author: 别回头丶 <312885991@qq.com>
|   Date:   Wed Jan 12 18:21:49 2022 +0800
|
|       master分支上修改a.txt
|
```

==注意==：当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。而解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

## 九、BUG分支

> 软件开发中，BUG 就像家常便饭一样。有了 BUG 就需要修复，而在Git中，由于分支是如此的强大，所以，每个 BUG 都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
>
> 但当你接到一个修复一个代号101的 BUG 任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在 `dev` 上进行的工作还没有提交，并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该 BUG ，怎么办？
>
> 幸好，Git 还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等解决完BUG后，再恢复现场继续工作。

```shell
# 比如，新建了一个b.txt文件，并且添加到了暂存区，但未提交到版本库中。查看此时状态
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   b.txt

# 将当前分支上的工作现场储存起来
git stash

# 再查看状态
$ git status
On branch master
nothing to commit, working tree clean

# 查看所有stash工作现场
git stash list

$ git stash list
stash@{0}: WIP on master: 76f2c95 合并冲突
stash@{1}: WIP on master: 76f2c95 合并冲突

# 恢复最近一次的工作现场
git stash apply (恢复现场，但恢复后，stash的内容不会删除，需要手动删除)
$ git stash apply
On branch dev
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   b.txt

git stash pop (恢复现场，并且会删除对应的stash内容)
$ git stash pop
On branch dev
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   b.txt

Dropped refs/stash@{0} (f89d3263c666988318456ba3a0676213920af788)

# 恢复指定的工作现场
git stash apply stash@{1}

# 删除最近一次的工作现场
git stash drop

# 删除指定的工作现场
git stash drop stash@{1}
```

## 十、标签管理

> 发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。`标签` 实际上就是对应某一个 `commit id` ，可以理解 `标签和版本号` 等价于 `域名和IP` 的关系。

```shell
# 在当前分支上创建标签（标签默认是打在最新提交的commit上）
git tag 标签名称
git tag v1.0

# 在指定commit上创建标签
git tag 标签名称 提交ID
git tag  v2.0  45655a0

# 创建带说明信息的标签
git tag -a 标签名称 -m 说明信息 

# 查看当前分支上的所有标签
git tag

# 查看对应的标签信息
git show 标签名称
git show  v1.0

# 删除对应的标签
git tag -d 标签名称
git tag -d  v1.0

# 推送指定的标签到远程仓库
git push origin 标签名称
git push origin v1.0

$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: Powered by GITEE.COM [GNK-6.2]
To https://gitee.com/LLS312885991/test.git
 * [new tag]         v1.0 -> v1.0

# 一次性推送全部尚未推送到远程的本地标签
git push origin --tags

# 删除远程仓库的标签
# 1. 先删除本地仓库的标签
git tag -d v1.0

# 2. 再从远程仓库中删除
git push origin :refs/tags/v1.0
```

