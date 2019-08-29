## git简介

Git是目前世界上最先进的分布式版本控制系统（没有之一）

#### 安装git 

自行百度，安装后可以输入 $ git 检查是否安装成功

#### 创建版本库

什么是版本库呢？版本库又名仓库，可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

- 选择一个合适的地方，创建一个空目录：

  ```
  $ mkdir learngit
  $ cd learngit
  $ pwd
  /Users/michael/learngit
  
  ```

- 第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

  ```
  $ git init
  Initialized empty Git repository in /Users/michael/learngit/.git/
  
  ```

- 瞬间Git就把仓库建好了，而且告诉你是一个空的仓库，且自动为我们创建了唯一一个`master`分支，可发现当前目录下多了一个`.git`的目录，如果没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见

#### 把文件添加到版本库

现在我们编写了一个`readme.txt`文件，把文件放到Git仓库

1. 使用命令`git add <file>`，可反复多次使用，添加多个文件，实际上就是把文件修改添加到暂存区
2. 使用命令`git commit -m <message>`，提交更改，实际上就是把暂存区的所有内容提交到当前分支

#### 版本回退

1. 查看历史提交记录：`git log`

   `git log`命令显示从最近到最远的提交日志，如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数

2. 回退到某一版本：`git reset --hard HEAD^`    或者  `git reset --hard `  '版本号'

   在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`

3. 版本恢复：用`git reflog`查看命令历史，以便确定要恢复到未来的哪个版本

#### 文件的修改

1. 查看工作区当前状态，都做了哪些更改：`git status`

2. 查看某个文件工作区和版本库里面最新版本的区别: `git diff HEAD -- file`

3. 丢弃某个文件的修改：`git checkout -- file`

   这里有两种情况：

   - 一种是`file`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态

   + 一种是`file`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态

4. 把暂存区的修改撤销掉，重新放回工作区: `git reset HEAD <file>` 

   `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区，`HEAD`表示最新的版本

5. 从版本库中删除某个文件: `git rm flie`

#### 远程仓库

1. 把已有的本地仓库与远程仓库关联：`$ git remote add origin '远程仓库地址'`
2. 把本地仓库的内容推送到GitHub仓库:  `$ git push -u origin master`
3. 克隆一个远程库到本地：`git clone '远程仓库地址'`

#### 分支的创建与操作

1. 查看分支：`git branch`

2. 创建分支：`git branch <name>`

3. 切换分支：`git checkout <name>`或者`git switch <name>`

4. 创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

5. 合并某分支到当前分支：`git merge <name>`

6. 删除分支：`git branch -d <name>`

7. 查看分支合并图：`git log --graph`

8. 把当前工作现场“储藏”起来，等以后恢复现场后继续工作: `git stash`

9. 查看被“储藏”起来的工作现场：`git stash list`

10. 恢复被"存储"起来的工作现场，有两个办法：

    - 用`git stash apply`恢复，但是恢复后，stash内容并不删除，需要用`git stash drop`来删除
    - `git stash pop`，恢复的同时把stash内容也删了

11. 分支合并：`git merge`

    合并分支时，如果可能，Git会用`Fast forward`模式，这种模式下，删除分支后，会丢掉分支信息

    如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息，可加上`--no-ff`参数

#### Feature分支

软件开发中，总有无穷无尽的新的功能要不断添加进来。添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个`feature`分支，在上面开发，完成后，合并，最后，删除该feature分支

如果要强行删除一个没有被合并过的分支：`git branch -D <name>`

#### 多人协作

1. 查看远程库的信息：`git remote`

   用`git remote -v`可显示更详细的信息

2. 把某分支上的所有本地提交推送到远程库：`git push origin <branch-name>`

   本地新建的分支如果不推送到远程，对其他人就是不可见的，如果推送失败，先用`git pull`抓取远程的新提交

3. 在本地创建和远程分支对应的分支，使用：`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致

4. 建立本地分支和远程分支的关联，使用：`git branch --set-upstream branch-name origin/branch-name`

5. 从远程抓取分支，使用：`git pull`，如果有冲突，要先处理冲突。

#### Rebase

1. 把本地未push的分叉提交历史整理成直线：`git rebase`
2. `git rebase`的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

#### 标签管理

1. 给某个分支的某个版本新建一个标签：	`git tag <tagname> <commit id>`  

   默认为当前版本HEAD`，也可以指定一个``commit i`

2. 指定标签信息: `git tag -a <tagname> -m "blablabla..."`

3. 查看所有标签: `git tag`

4. 推送一个本地标签: `git push origin <tagname>`

5. 推送全部未推送过的本地标签: `git push origin --tags`

6. 删除一个本地标签: `git tag -d <tagname>`

7. 删除一个远程标签: `git push origin :refs/tags/<tagname>`

#### 忽略特殊文件

1. 在Git工作区的根目录下创建一个特殊的`.gitignore`文件，把要忽略的文件名填进去，Git就会自动忽略这些文件
2. 有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被`.gitignore`忽略了，确实想添加该文件，可以用`-f`强制添加到Git：`git add -f file`

#### 配置别名

1. 如果敲`git st`就表示`git status`那就简单多了，操作命令：`git config --global alias.st status`

2. 当然还有别的命令可以简写，很多人都用`co`表示`checkout`，`ci`表示`commit`，`br`表示`branch`

   `git config --global alias.co checkout`
   `git config --global alias.ci commit`
   `git config --global alias.br branch`
   		
   ......等等以此类推

#### 搭建Git服务器

GitHub就是一个免费托管开源代码的远程仓库。但是对于某些视源代码如生命的商业公司来说，既不想公开源代码，又舍不得给GitHub交保护费，那就只能自己搭建一台Git服务器作为私有仓库使用。

搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的`apt`命令就可以完成安装。

- 第一步，安装`git`：

  `$ sudo apt-get install git`

- 第二步，创建一个`git`用户，用来运行`git`服务：

  `$ sudo adduser git`

- 第三步，创建证书登录：

  收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

- 第四步，初始化Git仓库：先选定一个目录作为Git仓库，假定是`/srv/sample.git`，在`/srv`目录下输入命令：

  ```
  $ sudo git init --bare sample.git
  ```

  Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以`.git`结尾。然后，把owner改为`git`：`$ sudo chown -R git:git sample.git`

- 第五步，禁用shell登录：出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：

  `git:x:1001:1001:,,,:/home/git:/bin/bash`

  改为：

  `git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell`

- 第六步，克隆远程仓库:

  `$ git clone git@server:/srv/sample.git`

  ### 管理公钥

   用[Gitosis](https://github.com/sitaramc/gitolite)

  ### 管理权限

  用[Gitolite](https://github.com/sitaramc/gitolite)

  

  ```
  
  ```

  

  ```
  
  ```

```
		


```













```


```

```



```

```

```





```



```

```

```





```


```







