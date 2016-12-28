```bash
# 查看版本
git --version

# 查看帮助
git help -a
git help -g
git --help

# 设置系统级别用户名和邮箱
# 具体配置写入/etc/gitconfig
git config --system user.name JeffXue
git config --system user.email contactmexkj@163.com

# 设置全局用户名和邮箱
# 具体配置写入~/.gitconfig
git config --global user.name JeffXue
git config --global user.email contactmexkj@163.com

# 设置本地用户名和邮箱
# 具体配置写入工程目录下的.git/config
git config --local user.name JeffXue
git config --local user.email contactmexkj@163.com

# 设置命令别名
git config --system alias.st status
git config --system alias.ci commit
git config --system alias.co checkout
git config --system alias.br branch

# git命令输出开启颜色
git config --global color.ui true

# 删除相关配置
git config --unset --global user.name
git config --unset --global user.email

# 重新修改最新的提交，改正作者和提交者的错误信息
git commit --amend --allow-empty --reset-author
# --amend 对刚刚的提交进行修补
# --allow-empty 允许空白提交
# --reset-author 将Author的ID同步修改

# 版本库创建三部曲
git init # 初始化仓库
git add  # 将文件添加到暂存区中
git commit # 将变更提交到版本库中

# 查看日志（有多种不同的格式）
git log --pretty=fuller 
git log --pretty=oneline
git log --oneline

# 通过--graph参数调用git log可以显示字符界面的提交关系同
git config --global alias.glog "log --graph"
git glog --oneline

# 只显示最近3次的日志
git log -3 --pretty=oneline

# 显示每次提交的具体变动
git log -p -l

# 显示每次提交的变更摘要
git log --stat --oneline

# 查看状态
git status
git status -s # 精简模式

# 理解Git暂存区（stage）
# Git会分为工作区、暂存区（statge）、版本库
# 文件.git/index包含文件索引和目录树(跟踪暂存区的内容)(二进制)
# 文件.git/HEAD则记录当前版本库工作分支，指向.git/refs/目录下的一个文件，该文件保存一个游标
# 文件的内容实际保存在git对象库.git/objects目录中

# 例子说明
# 工作区：新增修改一个文件后，该文件只是在工作区
# 暂存区：执行git add命令后，暂存区的目录树将被更新，同时工作区新增/修改的内容会被写入到对象库中的一个新的对象中，该对象的ID将被记录在暂存区的文件索引中
# 版本库：执行git commit后时，暂存区的目录树会写到版本库（对象库）中，HEAD会做相应的更新，只想提交时的暂存区的目录树（游标）

# 将工作区与暂存区比较
git diff 

# 将工作区和HEAD（当前版本库工作分支,非暂存区）相比
git diff HEAD

# 将暂存区与HEAD相比
git diff --cached
git diff --cached HEAD
git diff --staged

# 暂存区的目录树会被重写，会被HEAD指向的目录树所替换，但是工作区不受影响
git reset HEAD 

# 会直接从暂存区删除文件，工作区则不做出改变
git rm --cached 

# 会用暂存区全部的文件或指定的文件替换工作区的文件，这个操作很危险，会清除工作区中未添加到暂存区的改动
git checkout
git checkout -- 

# 会用HEAD指向的分支的全部/部分文件替换暂存区和工作区中的文件，这个命令极具危险
git checkout HEAD .
git checkout HEAD 

# 测试运行以便看看哪些文件和目录会被删除
git clean -nd

# 清除当前工作区中没有加入暂存区的文件和目录(具体可见git clean -h)
git clean -fd

# 不要使用git commit -a，会将本地所有变更的文件执行提交操作，包括本地修改和删除的文件，但不包括未被版本库跟踪的文件
# 会丢失git暂存区带给用户的最大好处：对提交内容进行控制的能力

# 保存当前工作进度
git stash # 运行后工作区尚未提交的改动（包括暂存区的改动）全部都不见了

# Git对象库
# 查看日志的详尽输出，每次提交的输出日志中有commit、tree、parent三个哈希值
# commit：本次提交的唯一标识
# tree：本次提交所对应的目录树
# parent：本次提交的父提交
git log --pretty=raw

# 通过提交对象之间的相互关联，可以很容易的识别出一条跟踪链
git log --graph --pretty=raw

# .git/refs是保存引用的命名空间的
# .git/refs/heads目录下的引用又称为分支

# 显示当前工作分支，带*号的表示这个分支是当前工作分支
git branch


# Git重置
# 重置到上一个老的提交上
git reset --hard HEAD^

# 重置到指定的提交上
git reset --hard 9e8a761

# 用reflog挽救错误的重置,默认情况下提供分支日志管理，见：git config core.logallrefupdates
# 查看分支指向的变迁，最新的改变放在最前面
git reflog show master

# 重置为两次改变之前的值
git reset --hard master@{2}

# git reset 用法
# 方法1：git reset [-q] <tree-ish> [--] <paths> 
#        重置指定路径下的文件为执行提交的内容，不会重置引用
# 方法2：git reset [--mixed | --soft | --hard | --merge | --keep] [-q] [<commit>] 
#        重置引用，根据不同的选项，可以对暂存区或工作区进行重置
#        --hard 替换HEAD的引用，替换暂存区，替换工作区
#        --soft 只替换HEAD的引用
#        --mixed 替换HEAD的引用，替换暂存区(不指定运行git reset时，默认则为mixed模式)

# Git检出
# git checkout 实际上就是修改HEAD本身的指向，该命令不会影响分支“游标”

# 基于某个分离头指针检出为一个分支
# 分离头指针状态指的就是HEAD分指针指向了一个具体的提交ID，而不是一个引用（分支）
git checkout -b new_branch_name 9e8a761 

# 将某个提交合并到当前分支上
git merge acc2f69

# git checkout 用法
# 方法1：git checkout [options] <branch>
#        如果省略，相当于从暂存区进行检出
#        -b 用于创建并切换到新分支
# 方法2：git checkout [options] [<branch>] -- <file>
#        用于指定分支文件覆盖工作区中对应的文件

# 例子说明
# 检出branch分支，更新HEAD，暂存区及工作区
git checkout branch

# 显示暂存区与HEAD的差异
git checkout
git checkout HEAD

# 使用暂存区文件覆盖工作区指定文件，相当于取消filename的本地修改
git checkout --filename

# 保持HEAD指向不变，使用branch所指向的filename替换暂存区和工作区中相应的文件
git checkout branch --filename

# 取消工作区的所有修改(相对于暂存区),相对危险
git checkout .

# 恢复进度
# 查看保存的进度
git stash list

# 从最近的保存的进度进行恢复，恢复前需要留意当前的工作分支
# 不适用任何参数，会恢复最新保存的工作进度
git stash pop

# 使用git stash
git stash # 保存当前工作进度(本地没有被版本控制系统跟踪的文件并不能保存进度)
git stash list # 显示进度列表
git stash pop [--index] [<stash>] # 恢复工作进度，并将恢复的工作进度从存储的工作进度列表中删除
git stash apple [--index] [<stash>] # 恢复工作进度，但不删除恢复的进度

# --index 除了恢复工作区的文件外，还尝试恢复暂存区
# --patch 会显示工作区和HEAD的差异，通过对差异文件的编辑决定在进度中最终要保存的工作区的内容，通过编辑差异文件可以在进度中排除无关内容
# -k或--keep-index，在保存进度后不会将暂存区重置
git stash [save [--patch] [-k|--[no-]keep-index] [-q|--quiet] [-u|--include-untracked] [-a|--all] [<message>]] 

git stash drop [<stash>] # 删除一个存储的进度
git stash clear  # 删除所有存储的进度
git stash branch <branchname> <stash> # 基于进度创建分支

# 设置tag
git tag -m "message" tagname

git describe

# 删除文件
git rm filename

# 将本地文件的变更全部记录到暂存区
git add -u

# 移动文件
git mv filename  newfilename

# 选择性添加
git add -i

# .gitignore用于配置忽略跟踪，可以放在任何目录中，作用范围是所处的目录及其子目录
# 忽略只对未跟踪文件有效，对于已加入版本库的文件无效
# 使用--ignored参数，才会在状态显示中看到被忽略的文件
git status --ignored -s

# 本地设置一个全局的独享的文件忽略列表
git config --global core.excludesfile ~/.gitignore

# Git 忽略语法
# * 代表任意多字符
# ? 代表一个字符
# [abc] 代表可选字符范围
# 名称最前面是路径分隔符/，表明要忽略的的文件在此目录下
# 名称最后面是路径分隔符/，表明要忽略的是整个目录
# 名称全面是感叹号! ，代表不忽略
# # 代表注释

# 文件归档
git archiva -o latest.zop HEAD
git archiva -o partial.tar HEAD src doc # 只对src和doc建立归档
git archiva --format=tar --prefix=1.0/v1.0 | gzip > foo-1.0.tar.gz # 基于里程碑v1.0建立归档,并为归档中的文件添加目录前缀

# gitk：图形化的git版本库浏览器软件
gitk  --all  # 显示所有分支
gitk --since="2 weeks ago" # 显示2周以来的所有提交
gitk v2.6.12.. include/scsi drivers/scsi # 显示某个里程碑以来针对特定目录的提交

# 其他工具
# gitg
# qgit

# 文件追溯吗，会逐行显示文件，在每一行的行首显示此行最早是什么版本引入，由谁引入的
git blame READM

# 将指定的提交在当前分支上重放，拣选后的新提交的哈希值是不同于所拣选的原提交的哈希值的
git cherry-pick <commit-ish>

# git rebase是对提交指定变基操作,即可以实现将指定范围的提交嫁接到另外一个提交之上
# git rebase --onto <newbase> <since> <till>
# 变基操作过程：
#（1）执行git checkout 切换到<till>
#（2）将<since>到<till>所标识的提交范围写到一个临时文件中
#（3）将当前分支强制充值到<newbase>（git reset --hard）
#（4）从保存的临时文件中的提交列表中，逐一按顺序重新提交到重置之后的分支上
#（5）如果遇到提交已经在分支中包含，则跳过该提交
#（6）如果提交过程中遇到冲突，则变基过程暂停，用户解决冲突后，执行git rebase --contine继续变基操作，或者执行git rebase --skip跳过此提交。或者执行git rebase --abort终止变基操作切换到变基前的分支上
# 添加-i参数接口进入一个交互界面进行变基操作
git rebase --onto C E^ F 

# 丢弃历史步骤：
#（1）创建新根提交
#（2）进行变基操作

# 方法1：
# 查看里程碑指向的目录树
git cat-file -p A^{tree}
# 直接从目录树创建提交
git commit-tree -m "message" A^{tree} 
# 执行变基，将里程碑A之后的提交全部迁移到新的根提交上
git rebase --onto 8f7f94b A master

# 方法2：
# 查看里程碑A对应的提交
git cat-file commit A^0
# 过滤掉parent开头，并保存到一个文件中
git cat-file commit A^0 | sed -e '/^parent/d' > tmpfile
# 将tmpfile作为一个commit对象写入对象库
git hash-object -t commit -w --tmpfile
# 执行变基，将里程碑A之后的提交全部迁移到新的根提交上
git rebase --onto 8f7f94b A master

# 反转提交:想修正一个错误历史提交的正确做法是反转提交，即重新做一次新的提交
# 将HEAD提交反向再提交一次
git revert HEAD

# 将指定版本库克隆到目录
git clone <repository> <directory>

# 克隆版本库，但不包含工作区，直接就是版本库的内容
# 一般约定裸版本库目录名以.git为后缀
git clone --bare <repository> <directory>
git clone --mirror <repository> <directory>

# 创建裸版本库
git init --bare

# 查看版本库中没有被任何引用关联的松散对象
git fsck

# 清理松散对象，而通过重置废弃的部分存在关联的提交会无法被清除
git prune

# 强制让之前的记录全部过期
git reflog expire --expire=now --all

# git gc对版本库进行一系列的优化动作
#（1）对分散在.git/refs下的文件进行打包，打包到文件.git/packed-refs中
#（2）丢弃90天前的reflog记录（git reflog expire --all）
#（3）对松散对象进行打包（git repack）
#（4）清除未被关联的对象。默认只清除2周以前的未被关联的对象（git prune --expire <date>）
#（5）其他清理（如对合并冲突的历史记录进行过期清理）
git gc
git gc --prune=now # 对所有未关联的对象进行清理

# 目前有以下命令后会自动指定git gc --auto命令
# git merge
# git receive-pack
# git rebase -i
# git am


# git通过检查推送操作是不是fast-forward推送，从而保证用户提交不会互相覆盖
# 一般情况下，推送只允许fast-forward推送
# fast-forward推送即要推送的本地版本库的提交是建立在远程版本库相应分支的现有提交基础上的

#强制推送，即使是non-fast-forward推送也会成功执行，强制刷新服务器中的版本
git push -f # 不一定是正确的解决方案

# 理性的工作协同要避免non-fast-forward推送
# 一旦向服务器推送后，如果发现错误，不要使用会更改历史的操作（变基、修补提交），而是采用不会修改历史提交的反转提交等操作
# 理性操作：执行git pull获取服务器最新的提交并和本地提交进行合并，合并成功后再向服务器提交

# 禁止non-fast-forward推送
#（1）修改/path/to/repos/shared.git
git --git-dir=/path/to/repos/shared.git receive.denyNonFastForwards true
# (2)通过钩子脚本进行设置

# 合并操作
git merge [options] <commit>

# 使用图形工具解决冲突
git mergetool

# 合并相关设置
git config --global merge.conflictstyle merge # 可用风格：merge, diff3
git config --global merge.tool kdiff3
git config --gloabl mergettool.kdiff3.path /path/to/kdiff3
# 如果所用的冲突解决工具不在内置的工具列表中，还可以使用mergetool.<tool>.cmd对自定义工具的命令进行设置
# merge.log 是否在合并提交说明中包含提交的概要信息，默认false

# 显示里程碑
git tag # -n<num>可以在显示里程碑的同时显示说明

# git log中显示提交对应的里程碑
git log --oneline --decorate

# 将提交显示为一个易记的名称
git describe

# 创建里程碑(轻量级，不记录谁创建，应使用带说明的里程碑)
git tag <tagname> [<commit>]

# 创建带说明的里程碑
git tag -m <msg> <tagname> [<commit>]

# 删除里程碑
git tag -d mytag

# 推送里程碑到远程版本库
git push origin mytag
git push origin refs/tags/*

# 必须显示推送tag
# 执行拉回操作，自动从远程版本库获取tag，并在本地版本库中创建
# 如果本地已有同名tag，默认不同步上游tag，需显示执行拉回操作

# 删除远程仓库tag
git push origin :mytag

# 显示分支
git branch

# 创建分支，不会自动切换
git branch <branchname>
git branch <branchname> <start-point>

# 删除分支
git branch -d <branchname>
git branch -D <branchname> # 强制删除
git push origin :branchname # 删除远程版本库的分支

# 重命名分支
git branch -m <oldbranch> <newbranch>
git branch -M <oldbranch> <newbranch> # 强制重命名
 
# 查看哪些提交领先
git cherry

# git pull = git fetch + git merge
# 当执行git pull 命令时，会和被跟踪的远程分支进行合并（或者变基）
# 当执行git push 命令时，会推送到远程版本库的同名分支中

# 注册远程版本库 origin
git remote add origin git@github.com:JeffXue/learnGit.git

# 显示已经注册的远程版本库
git remote -v

# 查看远程分支
git branch -r

# 从远程版本库中获取
git fetch # 会更新.git/refs/remotes/*
git fetch origin master # 会更新.git/refs/remotes/origin/master 

# 更改远程版本库地址
git remote set-url origin https://github.com/JeffXue/learnGit.git

# 更改远程版本库名称
git remote rename origin your-remote

# 获取所有远程版本库的更新
git remote update

# 删除远程版本库
git remote rm your-remote

# 本地新建分支中执行git push推送不会推送也不会报错
# 因为远程不存在同名分支，不会进行推送
# 如果本地其他分支在远程版本库有同名分支且本地包含更新的话，会对这些分支进行推送

# 使用变基而非合并操作，将本地分支的改动变基到跟踪分支上
git pull --rebase
git config branch.<branchname>.rebase true # 在<branchname>中执行git pull会默认采取变基操作
git config branch.autosetuprebase true # 基于远程分支建立本地分支时默认配置上面的rebase参数

# 设置不获取里程碑只获取分支和提交
git fetch --no-tags origin master

# 注册远程版本库时，避免将远程版本库的里程碑引入本地版本库
git remote add --no-tags hello-world file:///path/to/repos/hello-world.git


# Git版本库本身提供的安全机制，避免对版本库的破坏
# (1)用reflog记录对分支的操作历史
# (2)关闭non-fast-forward推送(可配置receive.denyNonFastForwards true 禁止一切non-fast-forward推送)
# (3)关闭分支删除功能(可配置receive.denyDeletes true 则禁止删除分支)

# 保存为一个补丁文件
git format-patch [<options>] [<since> | <revision-range>]

# 应用补丁文件
patch -pl xxxxxxxxxxxxx.patch

# 将mbox文件中的补丁全部应用到当前分支
git am user1-mail-archiva

```

