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
##
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