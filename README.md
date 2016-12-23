```bash
# 查看版本
git --version

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
git init
git add 
git commit

# 比较修改
git diff #实际比较的为工作区和暂存区的区别

# 查看日志（有多种不同的格式）
git log --pretty=fuller 
git log --pretty=oneline

# 查看状态
git status
git status -s # 精简模式

# 将工作区和HEAD（当前版本库工作分支,非暂存区）相比
git diff HEAD

# 将暂存区与HEAD相比
git diff --cached
git diff --staged





```

