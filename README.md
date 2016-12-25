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

```

