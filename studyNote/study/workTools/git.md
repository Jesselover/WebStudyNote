[总参考1](https://www.runoob.com/git/git-tutorial.html)
[总参考2](https://www.bootcss.com/p/git-guide/)

## 简要使用

#### 远程仓库

[git remote 命令 | 菜鸟教程 (runoob.com)](https://www.runoob.com/git/git-remote.html)

本地项目与远程仓库建立链接
```YAML
# 初始化本地仓库
git init 
# 与远程仓库第一次建立链接
git remote add orgin 远程仓库地址

```

```yaml
# 查看远程仓库简单名字
 -
# 查看远程仓库详情
git remote -v
git remote --verbose

```

```yaml
# 本地仓库已有内容，推送并连接到远程仓库
git init
# 设置 .gitignore 文件
git add .
git commit -m "xxxx"
git remote add orgin 远程仓库地址
# 将本地的master分支推送到orgin主机的master分支
git push -u origin master
# -u 参数相当于是让你本地的仓库和远程仓库进行了关联
#git push <远程主机名> <本地分支名>:<远程分支名>
```


## gitlap

- 自建代码托管平台、局域网代码托管中心

> TIP
> 一般需要**配置host** （公司内网映射）；直接覆盖掉原先的host，不需要重启


1. 访问
	1. ip地址
	3. 主机名（windows必须配置host映射）
	`[dutf_ssh · SSH Keys · User Settings · GitLab (mctech.vip)](http://git.mctech.vip:8080/-/profile/keys/1056)`
	`[Preferences · User Settings · GitLab (mctech.vip)](http://git.mctech.vip:8080/-/profile/preferences#behavior)`

## git

#### 概念

Git是一个开源**分布式**版本控制系统、一个版本管理工具，而GitHub、GitLab、Gitee等是基于git的仓库托管平台，让我们使用起来更方便

##### 集中式vs分布式

- **集中式**版本库集中存放在中央服务器，干活的时候，要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器

- **分布式**没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，工作的时候就不需要联网了，因为版本库就在自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了

  当然在实际使用分布式版本控制系统的时候，通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已

##### Git的工作流

- 工作区：add 之前的代码，自己的临时修改内容
- 暂存区：已经 add 进去，但还没有 commit 的
- 本地分支：已经 commit 提交的
- 远程分支：GitLab上可以看到的，项目组里所有人都能看见的

#### 安装与配置

在Windows上，可以从Git官网下载安装程序，然后按默认选项安装

安装完成后，在命令行里输入git -v能看到版本，在桌面点击右键能看到Git GUI Here和Git Bash Here的选项，说明安装成功！


##### 用户名和邮箱配置：

Git用户名和邮箱有全局配置和仓库配置之分，仓库配置优先级高于全局配置

全局配置：提交代码到gitlab上，如果没有为该项目单独配置用户名邮箱，则使用全局配置

```yaml
$ git config --global user.name “your Name”
$ git config --global user.email "your@xx.com"
```

单个仓库：

```yaml
$ git config user.name “your Name”
$ git config user.email "your@xx.com"
```

可使用git config (如果是全局就加上--global) --list查看当前配置

##### SSH配置：

首先在默认地址，一般是"C:\Users\Administrator"下的“.ssh”文件夹，查看有无文件，如果没有就是还没生成过SSH密钥；如果有就打开.pub文件，复制内容，填入GitLab里。

如果没有，执行命令生成：

```yaml
ssh-keygen -t rsa -C "your@xx.com"
```

GitLab里，点击右上角头像 -> “Edit Profile” -> “SSH Keys”，把自己电脑生成的SSH Keys填在这里，Title自定，Expires at不用填，默认永远不到期，填好后点击“Add key”即可

#### 使用

VS在 “视图” -> “终端” 里进行命令行操作，也可在对应文件夹里右键选git bash here来命令行操作

###### 新建项目和分支

```yaml
$ git clone git@xxxxx.git
$ git clone --branch <branchname> git@xxxxx.git
$ git add README.txt
$ git commit -m "本次提交的描述信息"
$ git branch
$ git branch -r 
$ git branch xx
$ git checkout -b xx
$ git checkout -b xx yy
$ git checkout xx
$ git branch -d xx
$ git push origin xx
$ git merge xx
```

1. 首先新建一个项目练习Git操作（不建议在正式项目中练习Git（！

   在gitLab新建空项目，创建好后会提示 “The repository for this project is empty” ；在自己电脑里找一个合适的目录，右键选择 git bash here，使用pwd命令可以显示当前目录；使用 git clone (clone with SSH 里的内容) 将远程项目克隆到本地

2. 完成后会显示warning，因为这是个空项目；新建的目录下会出现一个.git目录（不要试图修改该文件夹下的内容（！看不到.git 可能因为默认隐藏了，点击上方的 “查看” 按钮，确保 “隐藏的项目” 被勾选））
 

3. 添加文件

   右键点击项目目录，选择用VS打开，新建几个文件
   （建议使用VS或其他平台编辑文件包括文本文件，windows自带的记事本可能报错
   （编码格式建议选择UTF-8）

   1. 将 Visual Studio 设置默认编码格式为 **UTF-8**，[编码格式设置参考](https://blog.csdn.net/qq_41868108/article/details/105750175)

   2. 新建 **“.gitignore”** 文件，里面写上不需要提交的文件名或文件目录，如 [“.vs” 目录具体内容](https://www.cnblogs.com/liuqiyun/p/10196153.html)（vs自带，用于存储当前用户在解决方案中工作配置等内容）
      比如现在有了这几个文件，如果我们在.gitignore里加上first.txt，则first.txt文件会消失在列表里（如下图），以后提交也不会有它；
      
      .gitignore文件会被提交到远程，因此每一个拉取该项目的人都会遵循同一套提交规则，当然前提是不对.gitignore文件进行修改

   3. 建议新建 **“readme.md”** 文件，注意文件后缀，在VS中新建文件输入文件名时同时需要输入后缀；在readme中写上本项目的基本信息，相信每个项目都需要一些说明信息

4. 提交文件

   后续 “拉取提交推送” 部分有详细说明

5. 查看提交记录

   点击VS的 “视图” -> “Git存储库”，可在仓库里查看本地和远程仓库的提交记录，右键点击可查看每次提交的具体信息；在GitLab项目页点击 “History” 也可查看远程仓库的提交记录

   在VS端查看时，要先拉取才能看到最新的提交；在GitLab项目页可以看到全部

6. 创建分支

   项目页可以通过点击项目页的 “➕” -> "new branch" 创建新远程分支：

   本地可以用git push origin xx 将分支xx推送到远程仓库：

   - git branch xx 创建分支

   - git checkout -b xx 创建xx并切换过去，git switch -c xx 创建folk并切换过去，或通过git checkout -b xx origin/master 指定xx是通过远程master创建的

   远程即可看到新分支：
   
7. 分支操作
   - git merge xx 把xx分支合并到当前分支上
   - git branch -d xx 删除本地分支xx（删除前请确保当前不在xx分支，即切换到不想删除的分支（另，如果有error not full merged，则使用 git branch -D xx 强制删除分支xx ，即可
   - git push origin -d xx 删除远程分支xx
   - git branch 查看所有本地分支，git branch -r 查看所有远程分支，git branch -a查看所有分支
   - git checkout xx 切换分支，git switch xx 切换分支

###### 拉取提交推送

```yaml
$ git add xx.xxx
$ git add *
$ git commit -m "输入消息" 
$ git pull <远程主机名> <远程分支名>:<本地分支名>
$ git pull origin folk:folk
$ git pull origin folk
$ git push origin master
```

暂存add

- git add xx.xxx，把xxx文件放到暂存区
- git add * 暂存所有更改的文件，如果有不希望提交的文件可提前撤销更改，或使用stash（具体见本文 “其它” -> “stash” 部分）

提交commit

- git commit -m "输入消息" 把本次暂存的所有文件提交到本地仓库
- 要输入提交消息，通常是简短的一句话，让自己和项目中的其他人知道此次提交的大致信息

拉取pull

- 简单地说，pull 等于 fetch拉取 和 merge合并，git pull 将更新你的本地仓库至远程最新代码

- git pull origin folk:folk，把远程 folk 分支的更新拉过来与本地的folk合并，如果是和当前分支合并，则冒号后面的部分可以省略

- pull 时 git 会尝试自动合并，但如果出现一些情况，如远程和本地对同一文件做出改动，而出现冲突时，需要人肉合并来解决冲突；建议在VS里进行操作，可以比较直观的看到文件的冲突部分：

  双击查看 second.txt 冲突内容，选择好后点击 “接受合并” ，所有文件都合并好后，点击 “全部提交” ，由于开始的提交和合并后的提交算两件事，所以分别需要一个 “提交消息”

  （git pull 切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录

  （一般情况下，如果没有出现合并提示，就不会覆盖掉别人的代码


推送push

- git push origin xxx 推送到远程仓库的 xxx 分支
- 如果前面问题都解决了，这一步一般没啥问题
- VS里的 “同步”  ≈  “拉取” + “推送”  ≈  fetch + merge + push



###### 代码回滚

```
$ git checkout -- a.txt
$ git checkout -- . 
$ git reset HEAD 1.txt
$ git reset HEAD .
$ git reflog
$ git log
$ git reset --hard (id)
$ git reset --hard HEAD^
$ git reset HEAD^
$ git push origin HEAD --force
$ git git revert (id)
```

[代码回滚部分参考](https://www.jianshu.com/p/c55958563f5a)

- 回滚工作区的代码：

  - git checkout -- 1.txt 丢弃文件1.txt
  - git checkout -- . 丢弃全部文件

  丢弃，即回到上次在暂存区里的代码，如果没有暂存或commit，则回到上次pull后的代码

  对应VS里 “更改数” 下的 “撤销更改”

- 回滚暂存区的代码：

  - git reset HEAD 1.txt 回滚文件1.txt

  - git reset HEAD . 回滚全部文件

  仅改变暂存区，不改变工作区，相当于文件删掉了一次前面的备份

  对应VS里 “暂存更改” 下的 “取消暂存”

- 回滚本地仓库代码：

  - git reflog 查看所有分支的所有操作记录（包括已被删除的 commit 记录和 reset 的操作
  - git log 不能察看已经删除了的commit记录（在VS上使用会出现无法结束的bug）
  - git reset --hard (id) 回到哪个版本（用hard会删除代码，删除工作区的改动，撤销 commit 撤销 add；解决方法是 git reflog，找到自己想要的 commit，用 git reset --hard (id) 恢复
  - git reset --hard HEAD^ 回到最新的一次提交
  - git reset HEAD^ 代码保留，回到 git add 之前

- 回滚远程仓库代码：

  1. 通过git reset是直接删除指定的commit

     - git log
     - git reset --hard (id)
     - git push origin HEAD --force 强制提交一次，之前错误的提交就从远程删除

     如图，second4之后的已被删除

  2. 通过git revert是用一次新的commit来回滚之前的commit

     - git log
     - git revert (id)

  3. git revert 和 git reset的区别

     - git revert是用一次新的commit来回滚之前的commit，此次提交之前的commit都会被保留( 会有 两次 commit id)

     - git reset是回到某次提交，提交及之前的commit都会被保留，但是此commit id之后的修改都会被删除 ( 只有一次 commit id)，

       （但是用 reflog 还是可以查看某个被删除的提交，然后用reset --hard得到前面被删除的记录，如下图：

     ​	（然后正常推送即可恢复远程记录，这些记录的时间也不会被修改，如图：


###### 其它

1. **stash**：存储

   [stash部分参考](https://www.cnblogs.com/tocy/p/git-stash-reference.html)

   ```yaml
git stash
   git stash save "(name)"
git stash list
   git stash pop
git stash apply (id)
   git stash drop
   ```
   
   `git stash`，使用stash后工作区和暂存区的修改被储存，而工作目录中新的文件、被忽略的文件、以及本地仓库的不会被存储；可以使用 `-a` stash当前目录下的所有修改
   
   因此可以先将需要提交的文件提交，然后使用stash储存其余文件，再进行同步，同步结束后再将存储的内容出栈；出栈时自动合并，如有冲突会提示手动合并，如：
  
   建议使用`git stash save`，给每个stash加一个message，：
   stash储藏后会显示在这里：
   也可以使用`git stash list`查看现有stash：


使用`git stash pop`恢复之前缓存的工作目录，将缓存堆栈中的第一个stash删除，并将对应修改应用到当前的工作目录下

或使用`git stash apply`，将缓存堆栈中的stash多次应用到工作目录中，但并不删除stash拷贝

使用`git stash drop`，移除stash

使用`git stash show`查看stash的修改内容：

2. **cherry-pick**：将指定提交应用到其他分支

   [cherry-pick参考](https://ruanyifeng.com/blog/2020/04/git-cherry-pick.html)

   ```yaml
   git checkout master
   git cherry-pick f
   # 将提交f应用到master分支，参数是提交的哈希值或分支名，表示转移该分支的最新提交
   git cherry-pick A B # 一次转移多个提交(A、B)
   git cherry-pick A..B # 转移从 A 到 B 的所有提交(不含A)，A必须早于B
   git cherry-pick A^..B # (含A)
   git cherry-pick --continue
   ```

   代码冲突：

   ​	如果操作过程中发生代码冲突，Cherry pick 会停下来，让用户决定如何继续操作

   - **`--continue `** 自行解决代码冲突后，将文件加入暂存区，再使用continue命令

   - **`--abort`** 放弃合并，回到操作前的样子

   - **`--quit`** 退出，但是不回到操作前的样子（会保留显示冲突）：

3. **rebase**

   [rebase参考](https://blog.csdn.net/weixin_42310154/article/details/119004977)

   