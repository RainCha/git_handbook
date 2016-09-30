### Git速查手册
------
####  初始化git 仓库

```
    git init
```

#### 把文件修改添加到暂存区
git add test.txt test2.txt

#### 把暂存区的所有内容提交到当前分支。
git commit -m '添加了两个文件'

#### 修改文件后，查看状态
git status

#### 查看具体修改了什么内容(之后，就可以放心的add和commit了)
git diff test2.txt

#### 查看修改提交记录
git log --pretty=oneline

#### 版本回退（git commit后，想要撤销）
###### 当前版本HEAD，上一个版本就是HEAD^，上上一个版本就是HEAD^^,也可以加上版本号
git reset --hard HEAD^
git reset --hard commit_id

#### 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
git reflog

#### 撤销修改
###### 可以把暂存区的修改撤销掉（unstage），重新放回工作区。(git add后，想要撤销)
git reset HEAD test.txt
###### 可以丢弃工作区的修改
git checkout -- test.txt

#### 删除文件（在工作区删除了一个文件后）
###### 版本库中删除该文件，那就用命令git rm删掉，并且git commit：
git rm test.txt
git commit -m "remove test.txt"

###### 删错了工作区的文件，版本库里还有，所以要把误删的文件恢复到最新版本：
git checkout -- test.txt

####  配置git用户名
git config --global user.name "xxx"
git config --global user.email "xxxx@gmail.com"

#### 创建SSH key
ssh-keygen -t rsa -C "youremail@example.com"

-------

#### 关联本地库和github远程仓库
 git remote add origin git@github.com:example/learngit.git

`第一次使用加上了-u参数，是推送内容并关联分支推送成功后就可以看到远程和本地的内容一模一样,在此之前一定要git add和git commit ,否则会报错`
```
git push -u origin master  
// 如果远程仓库有文件和本地仓库不统一，上面这条命令就会出错，这是可以通过以下几种方式解决

// 1. 强制提交 -- 这样会使远程的修改丢失，一般是不可取的，尤其是多人协作开发的时候。
git push -u origin master -f 

// 2. push前先将远程repository修改pull下来,然后在提交
git pull origin master
git push -u origin master

// 3. 若不想merge远程和本地修改，可以先创建新的分支，然后push
git branch [name]
git push -u origin [name]
```

`下次只要本地作了提交，就可以通过命令：`
git push origin master

#### 先创建远程库，然后，从远程库克隆。
git clone git@github.com:example/test.git

---------

#### 创建分支
```
git branch dev           创建分支dev
git branch               列出所有分支，当前分支前面会标一个*号
git checkout dev         切换到dev分支
git checkout -b dev      创建并切换到dev分支
```

#### 合并分支
在dev分支上，修改了代码，并且commit了之后，想要合并到master分支
```
git checkout master
git merge dev            合并指定分支到当前分支
git branch -d dev        合并完成后，就可以放心地删除dev分支了
```

#### 创建本地分支develop后,推送至远程，并添加远程develop
```
git branch -d develop 
git push --set-upstream origin develop
```


#### 当两个分支都对同一文件作了修改，合并分支时就会产生冲突，需要进入到文件修改冲突部分，再次add&&commit
###### 使用下列命令查看分支的合并情况
git log --graph --pretty=oneline --abbrev-commit

#### 合并分支时，强制禁用Fast forward模式
###### Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息
git merge --no-ff -m "merge with no-ff" dev

#### 临时Bug分支

```
# Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
git stash

# 首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支
git checkout master
git checkout -b issue-101

# 修复完成后，提交
git add readme.txt
git commit -m "fix bug 101"

# 切换到master分支
git checkout master
git merge --no-ff -m "merged bug fix 101" issue-101
git branch -d issue-101

# 切换到dev分支，继续未完成的工作
git checkout dev

# 查看之前的工作现场
git stash list

# 恢复工作现场的同时把stash内容也删了（git stash apply不会删除stash内容）
git stash pop

## 多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
git stash apply stash@{0}


```

#### 查看远程库的信息（-v 详细信息）
git remote git remote -v

#### 推送分支(把本地的dev分支推送到远程origin 库)
git push origin dev                   



```
    #创建远程origin的dev分支到本地
    git checkout -b dev origin/dev
    
    # 在dev上继续修改，然后，时不时地把dev分支push到远程：
    git commit -m "xxxxxx"
    git push origin dev
    
    # 当两人同时对dev上的文件做出了修改，commit后,push就会出现冲突，此时就应该使用git pull把最新的提交从origin/dev抓下来
    git pull
    
    # 如果使用git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，
    # 根据提示，设置dev和origin/dev的链接,再pull
    git branch --set-upstream dev origin/dev
    git pull
    
    # 如果有冲突，要先处理冲突,再push
    git push origin dev
```

#####创建标签
git tag v1.0

##### 回退版本
```
git revert HEAD                 # 撤销前一次 commit
git revert HEAD^                # 撤销前前一次 commit
git revert commit     #（比如：fa042ce57ebbe5bb9c8db709f719cec2c58ee7ff）撤销指定的版本，撤销也会作为一次提交进行保存。

git reset commit    #（比如：fa042ce57ebbe5bb9c8db709f719cec2c58ee7ff） 回退到指定版本


```
