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
# $ git clone https://github.com/reactjs/redux.git 
```

## 分支管理
#### 创建与合并分支
创建dev分支，然后切换到dev分支：
```bash
$ git checkout -b dev
```
`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：
```bash
$ git branch dev
$ git checkout dev
```
然后，用`git branch`命令查看当前分支：
```bash
$ git branch
```
`git branch`命令会列出所有分支，当前分支前面会标一个 `*` 号。
切换分支用`$ git checkout name` 命令。

`git merge`命令用于合并指定分支到当前分支,把 `dev` 分支的工作成果合并到 `master` 分支上：
```bash
$ git merge dev
```
因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。
* 小结
Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

#### 解决冲突
Git用`<<<<<<<`，`=======`，`>>>>>>>` 标记出不同分支的内容。

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

用`git log --graph`命令可以看到分支合并图。
```bash
$ git log --graph --pretty=oneline --abbrev-commit
```
#### 分支管理策略
通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
合并分支时，请注意`--no-ff`参数，表示禁用`Fast forward`：
```bash
$ git merge --no-ff -m "merge with no-ff" dev
```
因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

#### BUG分支
创建一个分支issue-101来修复BUG，修复好后合并分支，但是，当前正在`dev`上进行的工作还没有提交(没有完成无法提交)：
幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
```bash
$ git stash
```
现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`maste`r创建临时分支：
```bash
$ git checkout master
$ git checkout -b issue-101
```
修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支.
用`git stash list`命令查看隐藏的分支工作现场。
```bash
$ git stash list
```
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash po`p，恢复的同时把stash内容也删了：
```bash
$ git stash pop
```
你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：
```bash
$ git stash apply stash@{0}
```

#### Feature分支
添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

但不需要这个新功能时，要将其销毁，当使用`$ git branch -d <name>` 进行销毁时，会提醒销毁失败。Git友情提醒，`feature`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用命令`git branch -D <name>`。
```bash
$ git branch -D feature-vulcan
```

#### 多人协作
当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`,或者，用git remote -v显示更详细的信息：
```bash
$ git remote
```
```bash
$ git remote -v
origin  git@github.com:kenve/learngit.git (fetch)
origin  git@github.com:kenve/learngit.git (push)
```
**推送分支：**
上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。
推送分支：`git push origin <name>`
```bash
$ git push origin dev
```
但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

* `master`分支是主分支，因此要时刻与远程同步；
* `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
* bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
* feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

**抓取分支**
多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。
当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的`master`分支。
现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地:
```bash
$ git checkout -b dev origin/dev
```
他就可以在`dev`上继续修改，然后，时不时地把`dev`分支push到远程。而碰巧你也对同样的文件作了修改，并试图推送：`$ git push origin dev`。推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：
```bash
$ git branch --set-upstream dev origin/dev
```
这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push。
多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin branch-name`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin branch-nam`e推送就能成功！
如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

**小结**

* 查看远程库信息，使用`git remote -v`；
* 本地新建的分支如果不推送到远程，对其他人就是不可见的；
* 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
* 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
* 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
* 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。 

## 标签管理
发布一个版本时，我们通常先在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。