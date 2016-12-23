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
```
