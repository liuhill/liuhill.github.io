---
title: git 如何强制覆盖本地文件
date: 2022-04-14 11:10:58
tags:
---
### 需求
在开发中使用Git的过程中，有时候会有一种需求，要从服务器拉取最新的状态，而本地进行了无关紧要的修改，这时候如果使用git pull命令，会提示本地有未缓存的修改。这时候就需要强制覆盖本地的改变。

### 一、最佳解决方案
重要提示：如果您有任何本地更改，将会丢失。无论是否有`--hard`选项，任何未被推送的本地提交都将丢失。
如果您有任何未被Git跟踪的文件(例如上传的用户内容)，这些文件将不会受到影响。
下面是正确的方法：
```
git fetch --all
```
然后，你有两个选择：
```
git reset --hard origin/master
```
或者如果你在其他分支上：
```
git reset --hard origin/<branch_name>
```
> 说明：
- git fetch从远程下载最新的，而不尝试合并或rebase任何东西。

- 然后git reset将主分支重置为您刚刚获取的内容。 --hard选项更改工作树中的所有文件以匹配origin/master中的文件

在重置之前可以通过从master创建一个分支来维护当前的本地提交：
```
git checkout master
git branch new-branch-to-save-current-commits
git fetch --all
git reset --hard origin/master
```
在此之后，所有旧的提交都将保存在new-branch-to-save-current-commits中。然而，没有提交的更改(即使staged)将会丢失。确保存储和提交任何你需要的东西。

### 二、次佳解决方案
```
git reset --hard HEAD
git pull
```
### 三、第三种解决方法
> 警告：git clean删除所有未跟踪的文件/目录，不能撤消。

> 有的时候只有clean -f无效。如果你没有跟踪DIRECTORIES，-d选项也需要：
```
git reset --hard HEAD
git clean -f -d
git pull
```
### 四、第四种方法
可行的方式是通过使用fetch和merge定义的策略。这应该能使你的本地修改保留下来，只要它们不是你试图强制覆盖的文件之一。

首先做一个你的改变
```
git add *
git commit -a -m "local file server commit message"
```
然后获取更改并覆盖，如果有冲突
```
git fetch origin master
git merge -s recursive -X theirs origin/master
```
`-X`是选项名称，"theirs"是该选项的值。如果存在冲突，则选择使用"their"更改，而不是"your"更改。

### 五、第五种方法
这是另外的思路，不建议上文的做法：
```
git fetch --all
git reset --hard origin/master
```
而是建议这样做：
```
git fetch origin master
git reset --hard origin/master
```
如果你打算重新设置原点/主分支，没有必要获取所有的遥控器和分支

### 六、第六种方法
看起来最好的办法是先做：
```
git clean
```
删除所有未跟踪的文件，然后继续使用通常的git ```
pull ...
```
### 七、第七种方法
所有这些解决方案的问题是，它们都是太复杂，或者更大的问题是，他们从Web服务器中删除所有未跟踪的文件，这是我们不想要的，因为总是有需要的配置文件服务器，而不是在Git仓库。

这是我们正在使用的最干净的解决方案：
```
# Fetch the newest code
git fetch

# Delete all files which are being added, so there
# are no conflicts with untracked files
for file in `git diff HEAD..origin/master --name-status | awk '/^A/ {print $2}'`
do
    rm -f -- "$file"
done

# Checkout all files which were locally modified
for file in `git diff --name-status | awk '/^[CDMRTUX]/ {print $2}'`
do
    git checkout -- "$file"
done

# Finally pull all the changes
# (you could merge as well e.g. 'merge origin/master')
git pull
```
- 第一个命令获取最新的数据。

- 第二个命令检查是否有任何正在添加到存储库的文件，并从本地存储库中删除那些会导致冲突的未跟踪文件。

- 第三个命令checks-out所有在本地修改的文件。

最后，我们将更新到最新版本，但是这次没有任何冲突，因为repo中的未跟踪文件不再存在，所有本地修改的文件已经与存储库中的相同。



> 作者：duyi324
链接：https://www.jianshu.com/p/1ac2e1f99166
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。