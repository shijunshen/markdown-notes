# 基本操作

```bash
git --version

# 首次安装git必须配置，否则无法提交代码
git config --global user.name 用户名	#设置用户签名，为了区分是谁提交的代码
git config --global user.email 邮箱	#设置邮箱

# 初始化本地库，要进入文件夹内运行，会生成.git文件
git init

# 查看状态
git status

# 添加暂存区 git追踪
git add xxx

# 提交本地库
git commit -m "日志信息" 文件名

# 查看log
git reflog	#精简log
git log		#详细log

# 版本穿梭
git reset --hard 版本号

```



# 分支操作

```bash
# 查看分支
git branch -v

# 创建分支
git branch 分支名

# 切换分支
git checkout 分支名

# 分支合并，站在当前分支下写要合并的分支，即把别的分支合并到当前分支上
git merge 分支名
```



# 团队协作(远程库)

## 我的demo库：

- http：https://github.com/shijunshen/demo.git

- ssh：git@github.com:shijunshen/demo.git

```bash
# 创建别名
git remote -v	#查看当前有哪些别名

git remote add 别名 链接
例如：git remote add demo https://github.com/shijunshen/demo.git
```

















