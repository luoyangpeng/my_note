# git 常用命令

### 设置Git的user name和email
$ git config --global user.name "test" <br>
$ git config --global user.email "test@163.com"
### 生成密钥
$ ssh-keygen -t rsa -C "test@163.com"
### 查看当前状态 
$ git status 
### 添加到本地版本库
$ git add .
$ git add --all
# 提交改变
$ git commit -m 'update'
### 提交并且加注释 
$git commit -am ''
### 推送到远程分支master
$ git push origin master
### 同步代码
$ git pull origin master
### 合并分支 test
$ git merge test
### 查看本地所有分支
$ git branch
### 新建分支 test
$ git branch -b test
### 查看所有分支
$ git branch -a
### 查看远程分支
$ git brach -r

$ git remote 不带参数，列出已经存在的远程分支

$ git remote -v | --verbose 列出详细信息，在每一个名字后面列出其远程url
### 添加远程仓库
$ git remote add 地址
### 切换分支 test
$ git checkout test
### 删除本地分支
$ git branch -D master develop 
### 删除远程分支
$ git push origin :branch_remote_name <br>
$ git branch -r -d branch_remote_name
### 忽略本地文件
$ git update-index --assume-unchanged PATH/FILE 
### 恢复跟踪
$ git update-index --no-assume-unchanged /path/to/file 

### 修改协议
$ git remote set-url 'git@xx'

### 更新忽略文件
$ git rm -r --cached .

### 打包两个版本的差异文件
$ git diff 61d2112 f3c0f99 --name-only | xargs zip update.zip
### 把修改的文件打包
git diff-tree -r --no-commit-id --name-only d18f9d5f17e190cfbb836a4acff2d96c0d466a2c | xargs tar -rf mytarfile.tar



### git 常用操作 
https://segmentfault.com/a/1190000018273794
