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
**待明确的参数**:`-u`,`master`

***
#### 从远程仓库克隆到本地
```
git clone git@github.com:your_git_path
```
