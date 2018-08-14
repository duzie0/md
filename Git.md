# Git

## 基本概念：

免费开源的分布式版本控制系统，用于敏捷高效的处理任何或大或小的项目版本的管理。

基本的 Git 工作流程如下：

1. 在工作目录中修改文件。
2. 暂存文件，将文件的快照放入暂存区域。
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。

## 使用git的优缺点：

### 优点：

- **分布式开发**，强调个体
- 服务器压力小
- 速度快，框架成熟，开发灵活
- 容易解决冲突
- **离线工作，管理代码成本低**
- 部署方便
- **分支机制良好**
- 免费

### 缺点：

- 资料少，学习周期长
- 不符合常规思维
- 代码保密性差

## Windows下安装Git：

- 下载[http://msysgit.github.io/](http://msysgit.github.io/)
- 安装
- 配置github的ssh秘钥：`ssh-keygen -t rsa -C "haosc1994@163.com"`
- 将.ssh目录下的id_rsa.pub内容复制到github
- 测试连接：`ssh -T git@github.com`
- 设置用户邮箱：`git config --global user.email "haosc1994@163.com"`
- 设置用户名：`git config --global user.name "duzie0"`
- 查看配置信息`git config --list` `git config <key>`


`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置

## 本地库操作：

- 在合适的目录下创建新的空目录
- 进入这目录使用`git init` 命令初始化本地库
- `git add <filename>` 把文件添加到仓库
- `git commit -m "<infomation>"` 把文件提交到仓库
- 关联本地库到远程库：`git remote add origin git@github.com:duzie0/myPro.git`

## 常用命令：

查看仓库当前状态：`git status`

工作区(work dict)和暂存区(stage)的比较：`git diff` 

暂存区(stage)和分支(master)的比较：`git diff --cached`

clone仓库：`git clone <url>` `git clone github@github.com:duzie0/md.git` 

会在当前目录clone下创建一个名为 “md” 的目录，并在这个目录下初始化一个 `.git` 文件夹，从远程仓库拉取下所有数据放入 `.git` 文件夹，然后从中读取最新版本的文件的拷贝。

**版本回退**

- 查看历史`git log`
- 日志参数`git log --pretty=oneline`
- 版本回退`git reset --hard HEAD^`
- 回退上三个版本`git reset --hard HEAD^^^` 或者`git reset --hard HEAD~3`
- 根据版本号`git reset --hard 1094a` 不必全完版本号
- 查询所有操作过的命令 `git reflog`

**git忽略文件**

`echo '*.log' > .gitignore` 

在本地库中创建`.gitignore` ,然后将需要忽略的文件添加到`.gitignore`

## 工作区与版本库：

![git-repo](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

创建Git版本库时，Git自动创建了唯一一个`master`分支，所以，`git commit`就是往`master`分支上提交更改。