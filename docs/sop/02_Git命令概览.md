---
description: git的一些基本命令整理
title: 🔧 git的一些基本命令整理
tag:
 - 配置
publish: true
readingTime: true
---
# git的一些基本命令整理
1. **git stash**
   1. 用来**临时存一下不想被提交的代码变更**，它的数据将被存在本地仓库 .git 文件下的 refs/stash中。
   2. **git stash save 'xxx'**: 储存变更
   3. **git stash list**: 查看储存区所有提交列表
   4. **git stash pop**: 弹出并应用最近的一次储存区的代码提交
   5. **git stash drop stash@{n}**: 删除某次储存记录
   6. **git stash clear**: 清楚所有 stash 信息
2. **git clone**
   1. **git clone xxx.git：**把一个仓库代码拉到本地
   2. **git clone xxx.git -b branch1**：指定仓库里的某个分支代码
3. **git init**
   1. 本地初始化一个 Git 仓库，就能开始对当前目录的改动纳入版本管理库。
   2. 不过本地 init 的仓库没法和远端进行交互，所以我们还是需要去 github/gitlab 创建一个远端仓库，然后关联一下，也就是 git remote 命令了。
4. **git remote**
   1. 用于**和远程仓库进行关系绑定处理**等等操作。
   2. git remote add: 添加一个远程版本库关联
   3. git remote rm: 删除某个远程版本库关联
   4. 比如我们本地有个初始化好的仓库，同时还有一个创建好的远程空仓库，那么我们就可以执行一下操作让他们关联起来：
   5. git remote add origin xxx.git：先添加到本地仓库
   6. git push -u origin master：表示把当前仓库的 master 分支和远端仓库的 master 分支关联起来，后面我们执行 push 或者 pull 都可以非常方便的进行操作了。
5. **git branch**
   1. git branch：查看本地所有分支信息
   2. git branch -r：查看远程仓库所有分支
   3. git branch -a：查看本地和远程仓库所有分支
6. **git checkout**
   1. git checkout -b branch1：创建并切换到指定新分支
7. **git add**
   1. git add [file1] [file2]: 添加一个或多个文件到暂存区
   2. git add .：把当前目录下得所有文件改动都添加到暂存区
   3. git add -A：把当前仓库内所有文件改动都添加到暂存区
8. **git commit **
   1. git commit [file1] ... -m [message]：将暂存区的内容提交到本地 git 版本仓库中
      1. -m 表示的是当前提交的信息
      2. -a 对于已经被纳入 git 管理的文件（该文件你之前提交过 commit），那么这个命令就相当于帮你执行了上述 git add -A，你就不用再 add 一下了；对于未被 git 管理过的（也就是新增的文件），那么还是需要你先执行一下 git add -A，才能正确被 commit 到本地 git 库。
9. **git rm**
   1. 比如我们项目中有个文件叫 .env，这个文件是一个私有的，不能被提交到远程的，但是我们不小心提交到了本地仓库中，这个时候我们把这个文件添加到 .gitignore 文件中，表示需要被 git 忽略提交，但是由于我们已经提交到本地仓库了，所以如果不先从 git 仓库删除是没用的。
   2. git rm .env：执行完这个命令就表示 .env 文件从 git 仓库中删除了，配合 .gitignore 就能保证以后所有的 .env 文件变更都不用担心被提交到远程仓库。
   3. git rm -r dist：如果我们要删除的是一个目录，那么加上 -r 参数就好了。
10. **git push**
   1. 接下来我们想要把刚创建好得分支推送到远端，一般来说我们可能会需要用到 git push，但我们这是个新分支，根本没和远端仓库建立任何联系，那么我们就需要加点参数，让他们关联上：
   2. git push --set-upstream origin branch1：推送分支并建立关联关系
   3. 如果创建的新分支在远程仓库已经有这个分支名怎么解决：
      1. 一种就是你本地的代码和远端代码没有冲突的情况下，并且你本地有新增提交，那么你可以仍然执行上述命令，这样就会直接将当前本地分支合远程分支关联上，同时把你的改动提交上去。
      2. 另一种就是本地分支和远端分支存在冲突，这个时候你执行上述命令就会出现提示冲突，那么接下来就需要你先把远端当前分支的代码拉下来，解决一下冲突了，就需要用到 git pull 命令了。
11. **git pull**
   1. git pull origin branch1：拉取指定远端分支合并到本地当前分支
   2. 了解完上面描述的 git pull，命令之后，其实这个命令也很好理解了，特定时候，可能我们只是想把远端仓库对应分支的变更拉到本地而已，并不想自动合并到我的工作区（你当前正在进行代码变更的工作区），等晚些时候我写完了某部分的代码之后再考虑合并，那么你就可以先使用 git fetch。
   3. fetch 完毕之后，我提交了自己当前工作去的变更到本地仓库，然后想合并一下远端分支的更改，这个时候执行一下 git merge origin/[当前分支名]（默认一般是用 origin 表示远端的分支前缀）即可。
12. **git merge**
   1. 合并指定分支代码到当前分支。一般来说，我们用的比较多的场景可能是，远端仓库 master 分支有变更了，同时这个时候我们准备提 MR 了，那么就需要先合一下 master 的代码，有冲突就解决下冲突，那这个时候我们可以做以下操作：
      1. 切到 master 分支，git pull 拉一下最新代码
      2. 切回开发分支，执行 git merge master 合并一下 master 代码
   2. 同理，上面介绍的 git merge origin/xxx 也是一样的用法。
13. **git log**
   1. 顾名思义，就是日志的意思，执行这个命令之后，我们能看到当前分支的提交记录信息，比如 commitId 和提交的时间描述等等，大概长下面这样：
   2. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/29155161/1688712548029-a1217e6f-220e-49c4-9a62-328dfdeb0a4a.png#averageHue=%23f5f4f3&clientId=ubf2d52ee-129e-4&from=paste&height=128&id=u23afb312&originHeight=176&originWidth=733&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=12203&status=done&style=none&taskId=u982822b2-94df-4ad9-9e57-f24e2d02cea&title=&width=533.0909090909091)
14. **git reset**
   1. 进行代码版本的回滚
   2. **git reset [--soft | --mixed | --hard] [HEAD]**
   3. **关于 HEAD：**
      1. HEAD 表示当前版本
      2. HEAD^ 上一个版本
      3. HEAD^^ 上上一个版本
      4. HEAD^^^ 上上上一个版本
      5. HEAD~n 回撤 n 个版本，这种也是更加方便的
   4. **参数解析**
      1. 以下解析均基于后接参数为 HEAD^，也就是git reset HEAD^。
      2. **--soft:** 重置你最新一次提交版本，不会修改你的暂存区和工作区。
      3. **--mixed:** 默认参数，用于重置暂存区的文件与上一次的提交(commit)保持一致，工作区文件内容保持不变。
      4. **--hard:** 重置所有提交到上一个版本，并且修改你的工作区，会彻底回到上一个提交版本，在代码中看不到当前提交的代码，也就是你的工作区改动也被干掉了。
   5. **举个例子**
      1. 我改动了我的 README 文件，在我们的工作区就产生了一次改动，但是这个时候还没有提交到暂存区，在 vscode 里会显示为工作区修改的标记
      2. 接着我们执行 git add，这个时候你查看暂存区，会发现这次改动被提交进去了，同时被 vscode 标记为已被提交至暂存区
      3. 然后再执行 git commit，这个时候就完成了一次提交
      4. 接下来我们想撤回这次提交，以上三种参数所体现的表现会是这样的：
      5. --soft：我们对 README 的更改状态现在变成已被提交至暂存区，也就是上面 2 的步骤。
      6. --mixed： 我们对 README 的更改变成还未被提交至暂存区，也就是上面 1 的步骤。
      7. --hard：我们对 README 的所有更改全没了，git log 中也找不到我们对 README 刚刚那次修改的痕迹。
      8. 默认情况下我们不加参数，就是 --mixed，也就是重置暂存区的文件到上一次提交的版本，文件内容不动。一般会在什么时候用到呢？
   6. **场景一（撤销git add）**
      1. **git reset (HEAD)**
      2. 如果 reset 后面不跟东西就是默认 HEAD。
   7. **场景二（撤销git commit）**
      1. 当你某个改动提交到本地仓库之后，也就是 commit 之后，这个时候你想撤回来，再改点其他的，那么就可以直接使用 git reset HEAD^。这个时候你会惊奇的发现，你上一版的代码改动，全部变成了未被提交到暂存区的状态，这个时候你再改改代码，然后再提交到暂存区，然后一起再 commit 就可满足你的需求了。
   8. **场景三**
      1. 某一天你老板跟你说，昨天新加的功能不要了，给我切回之前的版本看看效果，那么这个时候，你可能就需要将_工作区_的代码回滚到上一个 commit 版本了，操作也十分简单：
      2. **git log 查看上一个 commit 记录，并复制 commitId**
      3. **git reset --hard commitId 直接回滚**
15. **git reflog**
   1. 介绍：用来查看你的所有操作记录。
16. **git revert**
   1. 当然了，如果是针对 master 的操作，为了安全起见，一般还是建议使用 revert 命令，他也能实现和 reset 一样的效果，只不过区别来说，**reset 是向后的，而 revert 是向前的**，怎么理解呢？简单来说，**把这个过程当做一次时光穿梭，reset 表示你犯了一个错，他会带你回到没有犯错之前，而 revert 会给你一个弥补方案，采用这个方案之后让你得到的结果和没犯错之前一样。**
   2. 举个栗子： 假设你改了 README 的描述，新增了一行文字，提交上去了，过一会你觉得这个写了有问题，想要撤销一下，但是又不想之前那个提交消失在当前历史当中，那么你就可以选择使用 git revert [commitId]，那么它就会产生一次新的提交，提交的内容就是帮你删掉你上面新增的内容，相当于是一个互补的操作。
   3. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/29155161/1688719956724-d42894d2-8a41-4348-9531-223899ce2fc8.png#averageHue=%23f4f3f2&clientId=u5e16fbb4-c804-4&from=paste&height=92&id=udabec654&originHeight=127&originWidth=768&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=11065&status=done&style=none&taskId=u594adceb-9e66-4d19-a0e1-f3ad514cb59&title=&width=558.5454545454545)
   4. 这个时候，它会提示你可以把新的改动 push 上去了。
17. **git cherry-pick	**
   1. 其实对于我们工作中大部分场景下应该用不到这个功能，但是呢有的时候这个命令又能挽救你于水火之间，那就是当某个倒霉蛋忘记切分支，然后在 master 分支上改了代码，并且提交到了本地仓库中，这个时候使用git cherry-pick简直就是神器了。
   2. git cherry-pick：将执行分支的指定提交合并到当前分支。
   3. 比如我在 master 分支提交了某个需求的代码，同时还没提交到远程分支，那么你就可以先 git log 查看一下当前的提交，找到 master 分支正常提交之后的所有 commitId，然后复制出来，然后再切到你建好的开发分支，接着执行 git cherry-pick master commitId1 commitId2 commitId4。最后，再把你的 master 分支代码恢复到正常的提交上去。
18. **git tag**
   1. 顾名思义，也就是打标签的意思。一般可能会在你发布了某个版本，需要给当前版本打个标签，你可以翻阅 vite 的官方 git 仓库，查看它的 tag 信息，它这里就标注了各个版本发布时候的 tag 标签。
   2. 它有两种标签形式，一种是轻量标签，另一种是附注标签。
   3. **轻量标签**
      1. 创建方式：git tag v1.0.0
      2. 它有点像是对某个提交的引用，从表现上来看，它又有点像基于当前分支提交给你创建了一个不可变的分支，它是支持你直接 checkout 到这个分支上去，但是它和普通分支还是有着本质的区别的，如果你切换到了这个 tag "分支"，你去修改代码同时产生了一次提交，亦或者是 reset 版本，这对于该 tag 本身不会有任何影响，而是为你生成了一个独立的提交，但是却在你的分支历史中是找不到的，你只能通过 commitId 来切换到本次提交，看图：
      3. ![](https://cdn.nlark.com/yuque/0/2023/webp/29155161/1688726085373-8718b016-04b2-40df-a51b-8ed99043cbc5.webp#averageHue=%234b575b&clientId=u5e16fbb4-c804-4&from=paste&height=298&id=u6a4a3300&originHeight=424&originWidth=887&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u235d9b3e-a89b-4e3c-9072-16ee5d51f12&title=&width=624)
      4. 那如果你从其他分支通过 commitId 切换到这个改动上，它会提示你以下内容：
      5. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/29155161/1688726148273-c10f5edf-7bb6-4040-b279-b70135d94c8c.png#averageHue=%23f5f5f4&clientId=u5e16fbb4-c804-4&from=paste&height=264&id=u69449c9e&originHeight=363&originWidth=716&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=19182&status=done&style=none&taskId=u2f8f9801-767b-4f48-8f83-05e352c6ec3&title=&width=520.7272727272727)
      6. 大致意思就是你可以选择丢弃或者保留当前更改，如果需要保留的话直接使用下面的 git switch 命令创建一个新分支即可。
   4. **附注标签**
      1. 创建方式：git tag -a v1.0.1 -m "发布正式版 1.0.1"
      2. 附注标签是存储在 Git 数据库中的一个完整对象， 它们是可以被校验的，其中包含打标签者的名字、电子邮件地址、日期时间， 此外还有一个标签信息，并且可以使用 GNU Privacy Guard （GPG）签名并验证。
      3. 从概念上看，轻量标签更像是一个临时的标签，而附注标签更加正式一点，能够保留更多的信息。它创建的方式和轻量标签区别主要是 -a 和 -m 参数，如果你的 -m 参数不传，那么编辑器会让你手动填写。
   5. **推送标签**
      1. git push origin tagName
      2. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/29155161/1688726949441-d01a7980-f3cc-45f2-a5fb-ebeb4a2ee621.png#averageHue=%23f4f2f1&clientId=u5e16fbb4-c804-4&from=paste&height=180&id=ufb6e7368&originHeight=247&originWidth=553&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=21404&status=done&style=none&taskId=ub56847e2-a528-4542-a561-d13ad4310cf&title=&width=402.1818181818182)
      3. 附注标签和轻量标签都是可以被推送到远端的。
   6. **其他命令**
      1. 查看标签：git tag
      2. 筛选标签：git tag -l v1.0.1
      3. 删除标签：git tag -d v1.0.1
      4. 删除远程标签：git push origin --delete v1.0.2
      5. 另一种删除远程方式（表示将“:”前面空值替换到远程，也不失为一种方式）：git push origin :refs/tags/v1.0.1
19. **git rebase**
   1. **分支合并**
      1. 一般来说见得比较多的分支合并方式大多都是 git merge，少有用 git rebase，因为说实话 git rebase 一个用的不好就容易把代码给丢了，所以说如果你对 git rebase 不是很熟悉的话还是用 git merge 更靠谱点吧。那么，用 git rebase 进行分支合并一般又该怎么用呢？
      2. 首先假设你的 master 分支有一个 txt 文件，内容大致如下
```javascript
test1
test2
```

      3. 然后你从 master 拉了两个分支，一个 dev1，一个dev2，对这个 txt 文件改动分别如下：
      4. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/29155161/1688974935229-c1750969-02d2-4017-823a-bd22f69ae6a9.png#clientId=u12652769-0c0f-4&from=paste&height=278&id=uf0119429&originHeight=464&originWidth=978&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=21312&status=done&style=none&taskId=ue37969d7-6e18-42eb-8d77-58f0cec8762&title=&width=584.9091186523438)
      5. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/29155161/1688974957406-fe4386e0-ae25-458e-b7d1-f6b7af09086d.png#averageHue=%23f9f9f9&clientId=u12652769-0c0f-4&from=paste&height=337&id=ucfad441c&originHeight=464&originWidth=969&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=20132&status=done&style=none&taskId=u291500f9-64dc-4746-8f72-a746e801c09&title=&width=704.7272727272727)
      6. 分别给两个分支提交了两次 commit 用于修改各自的两行文本内容
      7. <br />

**

**

 

 

