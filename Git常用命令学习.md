> 前提：所有的版本控制系统，只能跟踪文本文件的改动。对于二进制文件，虽然也能由版本控制系统管理，但是没办法跟踪文件的变化，只能把二进制文件每次改动串起来(也就是只知道图片从100KB改为了120KB，但是到底改了什么，版本控制系统没办法知道)。

# 核心思想
- Git跟踪并管理的是修改，而非文件。

# 最常用的命令
```sh
git init         # 初始化
git add <file>   # 添加文件
git status       # 查看仓库当前的状态
git commit       # 提交(将暂存区的所有修改提交到分支)
git commit -m "" # 以引号内内容作为message提交
git diff a b     # 查看difference
git log          # 查看提交日志
git reflog       # 记录每一次提交的信息
git rm <file>    # 删除对一个文件的跟踪(即从版本库中删除一个文件)
```
# log信息的显示
```sh
git log                  # 显示完整提交信息
git log --pretty=oneline # 每行显示一个提交(commit-id是全写的)
git log --oneline        # 每行显示一个提交(commit-id是缩写的)
```
# Git的几个区
- 工作区(Work Area)：写代码的地方。
- 暂存区(Stage Area)：存放在`.git`文件夹下，暂存对于此仓库的修改。使用`git add`的时候，就是将要提交的所有修改放到暂存区。
# 撤销修改
- 工作区文件版本回退：`git restore <file>`。
- 暂存区文件版本回退：`git restore --staged <file>`。
# 远程库管理
```sh
# 关联一个远程库，其中origin是远程库的名字(默认习惯命名)
git remote add origin git@server-name:name:path/repo-name.git
# 例子：git remote add origin git@github.com:ArchieRen1248/learn_git.git

git remote -v # 查看当前配置有哪些远程仓库，-v选项查看每个别名的实际链接地址
git push -u origin master # 第一次推送master分支的所有内容(-u选项为了之后直接使用git push和git pull即可)
```
Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。
然而，使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。
# 分支管理
`HEAD`指向的就是当前分支：`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的。
## 常用命令
Git鼓励大量使用分支：
- 查看分支：`git branch`
- 创建分支：`git branch <name>`
- 切换分支：`git checkout <name>`或者`git switch <name>`
- 创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`
- 合并某分支到当前分支：`git merge <name>`
- 删除分支：`git branch -d <name>`
## 解决冲突
解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
用`git log --graph --oneline`命令可以看到分支合并图。
## FF模式
合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。
```sh
git merge --no-ff -m "merge with no-ff" dev
```
## 分支策略
- 首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
- 干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；
- 你和你的小伙伴们每个人都在`dev`扩展出去的分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。
- 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除。
## 临时隐藏工作区的修改
```sh
git stash      # 临时隐藏工作区修改(可不提交，临时去其他分支处理任务)
git stash list # 查看stash记录
git stash pop  # 恢复隐藏的内容，并把stash记录删除
git stash apply stash@{idx} # 恢复指定的stash
```
也可以用`git stash apply`恢复，但是恢复后，stash内容并不删除，需要用`git stash drop`来删除。
## cherry-pick命令
Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支。
## 未合并分支强制删除
使用`git branch -d feature-vulcan`删除分支，会因为未合并删除不了。可以用如下命令，强制删除一个分支(不过还是尽可能多留下来吧)。
```sh
git branch -D feature-vulcan
```
## 多人工作模式
多人协作的工作模式通常是这样：
1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！
如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。
# 创建远程仓库
参考：[搭建Git服务器 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)