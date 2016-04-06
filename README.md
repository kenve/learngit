# LearnGit
## this is test
## 查看log
`git log`命令显示从最近到最远的提交日志
```bash
git log
```
简化一行显示使用参数`--pretty-oneline`
```bash
git log --pretty-oneline
```
## 恢复指定版本
首先，Git必须知道当前版本是哪个版本，在Git中，用 `HEAD` 表示当前版本，也就是最新的提交`3628164...882e1e0`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。
```bash
git reset --hard HEAD^
```
返回后面的版本,版本号没必要写全，前几位就可以了，`405e7f6`Git会自动去找。
```bash
$ git reset --hard 405e7f6
```
在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到以前时，再想恢复到当前版本时，就必须找到当前的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：
```bash
$ git reflog
```

提交后，用`git diff HEAD -- test.md`命令可以查看工作区和版本库里面最新版本的区别：
```bash
$ git diff HEAD -- test.md
```

##　撤销修改

Git会告诉你，`git checkout -- file`可以丢弃工作区的修改：
```bash
$ git checkout -- test.md
```
就是让这个文件回到最近一次`git commit`或`git add`时的状态。
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

## 删除文件

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：
```bash
$ rm test.txt
```

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
```bash
$ git checkout -- test.txt
```
命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

## Github

在github新建仓库后，
新建仓库内容：
```bash
echo "# learngit" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/kenve/learngit.git
git push -u origin master
```
本地仓库的内容推送到GitHub仓库:
```bash
git remote add origin https://github.com/kenve/learngit.git
git push -u origin master
```
由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

要关联一个远程库，使用命令`git remote add origin https://github.com/kenve/learngit.git`

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改。

#### 克隆项目
假设远程库已经准备好了，下一步是用命令`git clone`克隆一个本地库：
```bash
$ git clone git@github.com:kenve/gitskills.git
#$ git clone https://github.com/reactjs/redux.git 
```