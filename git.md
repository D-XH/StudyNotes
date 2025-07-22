 [git](https://cloud.tencent.com/developer/article/2215632)

> token： 

### ssh公钥/私钥创建

```bash
ssh-keygen -t rsa -b 4096 -C "@qq.com"
```

## 命令

### 设置邮箱

```bash
git config --global user.name "用户名" # 设置全局的 Git 用户名
git config --global user.email "邮箱地址" # 设置全局的 Git 用户邮箱。

# 给当前仓库设置提交者邮箱和姓名
git config user.name "用户名" # 设置 Git 用户名
git config user.email "邮箱地址" # 设置 Git 用户邮箱。

git config -e # 针对当前仓库
git config --list
```

### 创建仓库

```bash
git init ./
```

### 克隆仓库

```bash
git clone 
	-b [branch] # 克隆指定分支
	--recursive # 克隆一个仓库及其所有子模块
	url
```



### 远程仓库

```bash
git remote add [alias] [url] # 添加一个远程仓库 eg.git remote add origin http://...
git remote # 查看已有的远程仓库
git remote -v # 显示具体远程仓库地址

# 本地仓库必须是空仓库
git fetch [alias] # 提取远程仓库的数据——相当于是从远程获取最新版本到本地，不会自动合并。
# fetch之后需要执行
git merge  [alias]/[branch] # 将服务器上的任何更新（假设有人这时候推送到服务器了）合并到你的当前分支。

git pull [options] [<repository> [<refspec>…]] # 相当于是从远程获取最新版本并merge到本地。

git push [alias] [local_branch] # 推送本地分支到远程仓库
git push [alias] [local_branch]:[remote_branch] # 推送到远程仓库某分支，没有就创建新分支

git remote rm [alias] # 删除add过的远程仓库
```

### 缓存修改

```bash
git add <filename> # 暂存文件
git checkout -- <fielname> # 放弃未暂存文件
```

### 缓存状态

```bash
git status # 查看文件的状态命令
git status -s # 简化版
# M-被修改 A-被添加 D-被删除 R-重命名 ？？-未被跟踪
```

### 查看缓存改动

```bash
git diff # 查看尚未缓存的改动
git diff --cached # 查看已缓存的改动
git diff HEAD # 查看已缓存的与未缓存的所有改动
git diff --stat # 显示摘要而非整个 diff
```

### 提交到本地仓库

```bash
git commit -m "第一次版本提交" # 将缓存区内容添加到仓库中
git commit -am "第一次版本提交" # 跳过commit前add这一步
```

### 还原

```bash
git reset HEAD <filename> # 取消add的文件
git reset HEAD^ # 回退所有内容到上一个版本
git reset HEAD~1 # 回退所有内容到上一个版本
git reset <commitID> # 回退到指定版本

# v2.33 引入
git restore <filename> # 将指定文件 <filename> 恢复到最新的提交状态
git restore --staged <filename> # 撤销已使用add缓存的文件
git restore --source=<commitID> <filename> # 将文件恢复到特定提交的状态
```

### 删除

```bash
git rm <filename> # 从已跟踪文件清单中移除
git rm -f <filename> # 强制删除【如果删除之前修改过并且已经放到暂存区域】

git ls-files	# 查看已经提交的文件
git rm --cached <file> # 仅是从跟踪清单中删除【把文件从暂存区域移除，但仍然希望保留在当前工作目录中】
```

### 分支管理

```bash
# v2.33 引入
git switch
#
git branch # 查看所有本地分支
git branch -r # 查看所有远程分支
git branch -a # 查看所有本地和远程分支
git branch <branchName> # 创建分支
git branch -b <branchName> # 创建并切换到该分支
git branch -m <newname>	# 改名

git checkout <branchName> # 切换分支
git checkout - # 切换到上一个分支
git checkout <commit-hash> # 切换到特定提交
git checkout tags/<tag-name> # 切换到该标签所指向的提交状态

git merge <branchName> # 把目标分支合并到当前分支
git branch -d <branchName> # 删除分支
```

### 提交历史

```bash
git log # 查看提交历史
	--oneline # 简洁版本
	--graph # 查看什么时候出现了分支、合并
	--reverse # 逆向显示所有日志
	--author=<username> # 查找指定用户的提交日志
	--since --before={YYYY-MM-DD} --until --after # 指定筛选日期
	--no-merges # 不显示合并提交
	--decorate # 显示相关联的分支／标签
```

### 标签

```bash
git tag -a <tag(eg.v1.0)> # -a 未创建带注释的标签
git tag -a v1.0 <commitID> # 追加标签
git tag # 查看标签
git tag -a <tagname> -m "某某标签" # 指定标签信息
```

### 代码撤销

```bash
--------撤销工作区的更改--------
git checkout -- filePath # 撤销工作区指定文件的更改，filePath,文件路径都可通过 git status查看
git checkout . # 撤销工作区所有更改
--------撤销暂存区的更改--------
git reset HEAD filePath # 撤销上次add指定的文件更改
git reset HEAD . # 撤销上次add的全部更改
```

### 子模块
```bash
--------子模块添加--------
git submodule add <submodule-url> <path>
--------子模块使用--------
git submodule init	
git submodule update

git submodule update --init --recursive
--------更新子模块--------
cd <path>
git pull ...
--------删除子模块--------
rm -rf <path>
vi .gitmodules		# 删除项目目录下.gitmodules文件中子模块相关条目
vi .git/config		# 删除配置项中子模块相关条目
rm .git/module/*	# 删除模块下的子模块目录，每个子模块对应一个目录，注意只删除对应的子模块目录即可
####再添加子模块时，如果有报错
git rm --cached 子模块名称
```

### 复制最后一次提交的分支

```bash
# 创建一个新分支。加上一个--orphan参数，加上这个参数之后创建的分支有点特殊，他只有最后一个版本，而不是把所有的版本都复制过来，严格来说创建出来的不是分支，但很像分支
git checkout --orphan new_branch
# 将这个特殊的分支里面的文件都添加进来
git add -A  && git status
# 提交到一个版本当中
git commit -m "new version"
# 将原来的分支删除
git branch -D old_branch
# 改名
git branch -m develop
```



