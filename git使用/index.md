# Git使用


### 1. 子仓同步主仓
1. 新增主仓代码库
`git remote add upstream 'xxx.git'`

2. 拉取主仓代码至本地master分支
`git pull upstream master`

3. 提交本地代码至远程子仓
`git push`

### 2. 强制push，覆盖远程仓库
1. 回退本地代码至前一次提交
`git reset --soft HEAD~1`

2. 强制push，覆盖远程
`git push -u origin master -f`
