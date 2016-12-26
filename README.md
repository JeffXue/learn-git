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




```

