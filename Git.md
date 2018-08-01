# Git

## 基本概念：

## 使用git的优缺点：

### 优点：

分布式

### 缺点：

## 工作区与版本库：

![git-repo](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

## Windows下安装Git：

- 下载[http://msysgit.github.io/](http://msysgit.github.io/)
- 安装
- 配置github的ssh秘钥：`ssh-keygen -t rsa -C "haosc1994@163.com"`
- 将.ssh目录下的id_rsa.pub内容复制到github
- 测试连接：`ssh -T git@github.com`
- 设置用户邮箱：`git config --global user.email "haosc1994@163.com"`
- 设置用户名：`git config --global user.name "duzie0"`

