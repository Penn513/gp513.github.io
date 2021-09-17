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

### 3. 下载大仓库
1. clone最新代码
`git clone xxx.git --depth=1`

2. 历史记录
`git fetch --unshallow`

### 4. 子模块
1. 方式一：直接clone
`git clone --recursive xxx.git`

2. 方式二： 已经clone后下载子模块
`git submodule update --init --recursive`
