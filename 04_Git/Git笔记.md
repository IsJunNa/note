## 版本命令

### 设置用户名称

```
$ git config --global user.user "用户名"
```



### 设置用户邮箱

```
$ git config --global user.email "邮箱"
```



### 初始化本地库

```
$ git init
```



### 查看本地库状态

```
$ git status
```



### 添加到暂存区

```
$ git add 文件名
```



### 提交到本地库

```
$ git commit -m "版本名" 要提交的文件名 
```



### 查看历史记录

```
$ git reflog 
```



### 查看详细的历史记录

```
$ git log 
```



### 版本梭

```
$ git reset --hard 要穿梭到的版本号
```



## 分支命令

### 查看当前分支

```
$ git branch -v
```



### 创建新的分支

```
$ git branch 新的分支名称
```



### 切换当前分支

```
$ git checkout "要切换的分支名"
```



### 合并分支

```
$ git merge 要被合并的分支名
```



## 远程托管命令

### 查看已有的别名

```
$  git remote -v
```



### 创建别名

```
$  git remote add 别名 别名所替代的
```



### 推送到远程仓库

```
$ git push 推送地址 要推送的分支名
```



### 拉取远程仓库代码到本地

```
$ git pull 拉取地址 要拉取的分支名
```













