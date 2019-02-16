# Git的常用操作（本地常用操作）

#### 基本概念
* 工作区
电脑中的实际文件
* 暂存区（stage）
git中的缓存
* 版本库
文件一旦提交到版本库，就意味着创建了一个还原点
#### 创建Git仓库

* `git init`
```shell
mkdir 仓库名
pwd #查看当前路径  
cd 仓库名
git init #初始化仓库
```

#### 提交版本变动
* `git add`
```shell
git add readme.txt  #图省事可以git add . 添加全部文件
```
把工作区的文件改动添加到暂存区，改动是指当前工作区与最后一次提交的版本作比较
* `git commit -m 'xxx'`
```shell
git commit -m 'wrote a readme file' #把暂存区的文件提交到仓库
```
类似于游戏存档，注意是把暂存区的改动添加到了版本库，而暂存区是依靠`git add`来从工作区添加的
#### 查看状态
`git status`  查看当前仓库的状态
`git diff 文件名` 查看某个文件的改动  
`git log` 查看从最近到最远提交的日志（存档点）
#### 版本回退
参数说明：
`HEAD`:
>`HEAD`代表当前版本（最后一次提交的版本），`HEAD^`代表上一个版本（按照log顺序）,依次类推`HEAD^^`，为了避免版本数过多，也可以用数字表示选则回顾前多少个版本，比如回顾3个版本，`HEAD~3`（等价于`HEAD^^^`）

回滚命令：
`git reset --hard HEAD`
类似于相对路径
`git reset --hard xxx(log中的commit id)`
类似于绝对路径（xxx不用写全，写前几位就可以）
**注意`git reset --hard xxx`中的`--hard`是没有空格的**

恢复命令：
回滚到之前的版本，后悔了，想复原
思路：查找到对应版本的commit id，但是用`git log`不会看到当前版本之后的版本，所以要用 `git reflog`命令

#### 撤销修改
与版本回退的区别是，版本回退恢复的是所有文件，而撤销修改只是撤销掉一部分的修改
* ` git checkout -- filename`
注意`checkout`后面一定要有`-- 文件名`，否则是切换分支。
效果：
1.工作区的文件未添加到暂存区
*效果同版本回退一样*
2.工作区的文件已添加到暂存区
*撤销修改到最后一次添加暂存区的状态*
**修改了工作区的内容**


* `git reset HEAD 文件名`
清除指定文件在暂存区中的内容
**只是清空了暂存区中某个指定的文件，工作区没有变动，修改工作区要靠`git checkout -- filename`**

**问题**：若文件已经`add`到暂存区，想撤销修改到`add`之前该怎么办？
思路：先清空该文件的暂存区，再撤销工作区的修改
解决：`git reset HEAD 文件名` 把指定文件的暂存区中的修改全部撤销，然后`git checkout -- 文件名`，使工作区的文件恢复到最近一次`commit`或`add`（然而暂存区已通过`git reset HEAD 文件名`清楚了）的状态

#### git删除
`git rm 文件名`
`git commit -m 'xxx'`
删除版本库中的某个文件
`git rm`其实与`rm`是等价的

***

## git远程操作

#### 前期准备
1. 创建ssh
 `ssh-keygen -t -rsa -C "your email@example.com"`
执行后在`用户主目录`找到.ssh目录，里面有`id_rsa`和 `id_rsa.pub`，我们要用的是`id_rsa.pub`

1. 登陆github，配置ssh
    * 打开右上角头像中的`Settings`
    * 点击左侧`SSH and GPG keys`选项
    * 点击按钮`New SSH key`，添加`id_rsa.pub`

至此，准备工作结束

****
#### 连接远程仓库
1. 从github代码仓的`clone or download`按钮获取`ssh`路径
1. 在本地运行`git remote add orgin <ssh路径>`
    ```shell
    git remote add origin git@github.com:your_ssh_path
    ``` 
    其中`origin`是你给所连接的远程仓库取的别名
 ***
#### 本地仓库推送到远程仓库
```
git push -u origin master
```
`git push`用于把本地推送至远程仓库，`origin`是之前连接远程仓库时给该仓库取的别名，
##### 参数解释
`-u`:
>更新`git push`命令的默认参数，在运行`git push -u origin master`后，下次再直接运行`git push`命令，就相当于运行的是`git push origin master`

`master`：
>推送的本地分支

`origin`:
>推送至指定的主机，`origin`是执行`git remote add`命令时给远程主机起的别名

***
#### 从远程仓库克隆到本地
```
git clone git@github.com:your_git_path
```
