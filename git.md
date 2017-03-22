# git 常用命令

### 设置Git的user name和email
$ git config --global user.name "test" <br>
$ git config --global user.email "test@163.com"
###生存密钥
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
