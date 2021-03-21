<h1 align ='center'> <span style='color:red'> Git 笔记 </span> </h1>

## 

- 创建一个新的提交记录：`git commit`;

- 创建一个到名为 `newImage` 的分支: `git branch newImage`;

- 切换到`newImage`分支：`git checkout newImage; git commit`;

- 创建一个新的分支`newBranch`同时切换到新创建的分支:`git checkout -b newBranch`

- 合并两个分支:`git merge`，例如：把 `bugFix` 合并到 `master` 里：

  ​											`git checkout master;git merge  bugFix;` 

- 第二种合并分支的方法是 `git rebase` ，例如：把当前分支（`bugFix`）移动到main分支：`git rebase main`;`git checkout main`;`git rebase bugFix`;

> HEAD 是一个对当前检出记录的符号引用 —— 也就是指向你正在其基础上进行工作的提交记录。如果想看 HEAD 指向，可以通过 `cat .git/HEAD` 查看。HEAD 通常情况下是指向分支名的，分离的 HEAD 就是让其指向了某个具体的提交记录而不是分支名。
>
> > **HEAD永远指向当前检出记录，即激活的节点；**

- 分离HEAD：使用`git checkout 提交记录哈希值`可以分离HEAD（使用`git log`来查看提交记录的哈希值）。*使用哈希值很不方便，因此Git引入了`相对引用`*。

- 相对引用：使用`^`向上移动一个提交记录，使用`~num`向上移动多个提交记录（都是基于当前操作的节点）；例如，切换到main的父节点：`git checkout main^`(也可以使用HEAD作为相对引用的参考：`git checkout HEAD^`)。

- 强制修改分支位置：使用 `-f` 选项让分支指向另一个提交。例如:`git branch -f main HEAD~3`会将 main 分支强制指向 HEAD 的第 3 级父提交。

- 撤销变更：和提交一样，撤销变更由底层部分（暂存区的独立文件或者片段）和上层部分（变更到底是通过哪种方式被撤销的）组成。主要有两种方法用来撤销变更 `git reset`和 `git revert`。

  - `git reset` 通过把分支记录回退几个提交记录来实现撤销改动。本地分支中使用 `git reset` 很方便，但是这种“改写历史”的方法对大家一起使用的远程分支是无效的。

    > `git reset`的原理是修改HEAD的指向，将其指向之前存在的某个版本。

  - 为了撤销更改并**分享**给别人，我们需要使用 `git revert`

    > `git revert`的原理是**反做**某一版本（其他版本的修改都会保留，甚至可以反做中间某个版本，但这个版本之后的其他修改会保留，不会被反做）。

> `pushed` 是远程分支，`local` 是本地分支





# 廖雪峰Git教程

> 设置自己的信息：
> git config --global user.name "WZZ"
> git config --global user.email "752371893@qq.com"

### 本地仓库

1. 选择一个文件夹创建仓库：
   1. `cd F:/LearnGit`; 
   2. `git init`;
2. 往仓库添加信息：`git add readme.txt`(readme.txt是一个已经存在于仓库路径中的文件)
3. 提交：`git commit -m "notation"`(引号中为本次提交的说明)
4. 查看变更日志：`git log [--pretty=oneline]`
5. 回退：`git reset --hard HEAD^`
6. 查看操作历史：`git reflog`

> `git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

7. 查看仓库及暂存区状态：`git status`

   + `-s`:以短格式输出[short]
   + `-b`:以短格式显示分支和跟踪信息[branch]

8. 查看工作区和版本库里面最新版本的区别：`git diff HEAD -- readme.txt`

9. 撤销工作区的修改（==未提交==）：`git checkout -- readme.txt`

   1. `readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
   2. 一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

   > 总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态;
   >
   > `git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令;

10. 撤销暂存区的修改（把暂存区的修改重新放回工作区）：`git reset HEAD readme.txt`或者`git restore --staged  readme.txt`

11. 删除文件：先手动删除文件，然后`git rm readme.txt`;`git commit -m "rm readme.txt"`（和添加文件刚好相反）

    + 删除暂存区的文件：`git rm --cached <filename>`

    ---

+ **`git reset <>`**:
  + `git reset 文件名`：撤销指定文件的`git add`操作，即这个文件在**暂存区**的修改；
  + `git reset`:撤销之前所有的`git add`操作，即在**暂存区**的修改；

### 远程仓库

+ 创建 远程仓库

  1. 创建SSH Key：`ssh-keygen -t rsa -C "youremail@example.com"`

  > 本地Git仓库和GitHub仓库之间的传输是通过SSH加密的

  2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面进行设置。参考[远程仓库](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416 "廖雪峰")
  3. 在GitHub上创建一个仓库
  4. 与本地仓库关联：`git remote add origin git@github.com:用户名/仓库名.git`
  5. 第一次将本地仓库master分支推送到GitHub上：`git push -u origin master`
     此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

+ 从远程库克隆

  1. 在github上创建一个仓库，名称为`gitskills`;

  2. 使用`git clone`命令克隆到本地：

     >  `git clone git@github.com:wzz752371893/gitskills.git`

### 分支管理

> + *head 指向当前分支，使用cat .*git*/*HEAD查看*当前HEAD指向*
> + *每次提交，master分支都会自动向前移动一步*



+ 查看当前分支情况：`git branch`

+ 创建分支

  + 创建分支：`git branch <name>`
  + 创建并切换到分支：
    + `git branch -b <name>`或者<u>**`git switch -c <name> `**</u>

  > 创建一个新分支，本质上是创建一个指向master的指针，我们需要把head指向新建的分支，才能在新的分支上工作
  >
  > git 分支改变本质上就是移动指针指向

+ 合并分支

  + 合并<name>分支到当前分支：`git merge <name>`
  
  + `git merge -- no-ff -m "commit msg" <name>`:不使用fast forward模式合并并提交
  
  + > **fast forward模式合并完成后两个分支指向的指针相同，对任何一个分支修改斗都会同步影响另外一个分支，所以合并完之后，一定要删除子分支**
  
+ 删除分支

  + `git branch -d <name>`
  + 强制删除：`git branch -D branchname`
  
+ 切换到已有分支

  + `git checkout <name>`或者<u>**`git switch <name>`**</u>
  + 切换分支，本地文件（工作区）也会同步改变。
  
+ 当前工作区正在修改（还不能添加到暂存区），需要切换分支进行另外一个工作时：

  + 使用`git stash`保存现场，然后使用`git switch <name>`切换分支；

  + 恢复现场：`git switch <现场分支>`，然后`git stash pop`

  + > 注：可以多次用命令git stash 保存工作现场，然后用git stash list 查看保存的列表，然后用 git stash apply恢复。   用 git stash apply恢复后，保存的现场并不删除，需要用git stash drop 删除。


+ 解决冲突

   	当合并产生冲突时，可以进行如以下操作：

  + 使用`git status`查看冲突状况（哪些文件冲突）
  + 使用`cat filename`查看冲突内容（文件里的哪些行）


  + 可以终止合并`git merge -- abort`
  + 也可以手动解决冲突后再次提交`git commit`

+ 分支管理策略

+ Bug分支

+ Feature分支

+ 多人协作

+ Rebase







