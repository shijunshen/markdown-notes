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

git add xx 命令可以将xx文件添加到暂存区，如果有很多改动可以通过 git add -A . 来一次添加所有改变的文件。
# 注意 -A 选项后面还有一个句点。 git add -A 表示添加所有内容， git add . 表示添加新文件和编辑过的文件不包括删除的文件; git add -u 表示添加编辑或者删除的文件，不包括新添加的文件



# 提交本地库
git commit -m "日志信息" [文件名] #不加wen

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

# 推送本地库到远程库
git push 别名/链接 分支名

# 拉去远程库到本地库
git pull 别名/链接 分支名

# 克隆远程库，进入你要存放代码的文件夹
git pull 别名/链接 [文件夹名字]	#这里的文件夹名字默认是远程库的名字，也可以修改成你自己想要的名字
## 克隆做三件事
#	1、拉取代码。2、初始化本地仓库。3、自动为你创建别名

```

















