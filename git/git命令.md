# git命令

## 常见问题

### MERGING

```
git reset --hard head
```

### 两次提交记录修改为一条

```
git rebase -i HEAD~2
s 第二个pick改为s
:x 保存
dd 删除
:x保存
```

### 删除远端分支(temp)

```
git push --delete origin feature/mark_blockchainweilai
```

### 删除本地分支

```
git branch -d temp
git branch -D temp
```

### push 代码到远端

```
git push origin temp
```

### 从远端pull代码

```
git pull origin develop
```

### 切换到develop分支

```
git checkout develop
```

### git clone

```
git clone https://github.com/Ebookcoin/ebookcoin.git
```

### 查看远程仓库地址

```
git remote -v
```

### 查看远程分支

```
git branch -a
```

### 克隆远程分支

```
git checkout --track origin/feature/nk_joe_blockchain
```

### 查看分支

```
git branch
```

### 创建分支

```
git branch develop
git checkout develop
```

或者：

```
git checkout -b dev
```

### 切的分支更新

```
git pull origin develop
commit 自己分支代码
git rebase develop
git push origin feature/mark_blockchainweilai
```

### git 回滚到上一个版本

```
git reset --hard HEAD~1  
```

### git merge

```
git merge --no-ff origin/`feature/ch_blockchain_sched
```

### git stash

```
git stash save "work in progress for foo feature"
$do some work
git stash pop
```

### git 重命名分支

```
重命名本地分支：
git branch -m dev develop

删除远程分支：
git push --delete origin dev

推送本地分支：
git push origin develop
```

