# Ubuntu

## 文件操作

### 创建目录：

mkdir :

`mkdir <目录名>` 在当前目录下创建一个目录

`mkdir <目录名> <目录名> <目录名>` 创建多个目录

`mkdir -p <目录名>/<目录名>/<目录名>` 创建多级目录

### 删除目录文件：

`rmdir <目录名>` 删除目录（只能是空目录）

`rm <文件名>` 删除文件

`rm -rf <目录名>` 强制遍历并删除目录下所有内容

### 创建文件：

`touch <文件名>` 创建文件

### 进入目录：

`ls ` 列出当前目录中的所有文件

`ls -a` 

`pwd` 当前目录

`cd <目录名>` 进入某一级目录

`cd ..` 回到上一级目录

## 权限问题

## 系统用户

## 压缩解压

## shell

## 开机自启动

## 防火墙

## 进程管理

### 查询端口占用

`lsof -i <端口>` 查看当前使用该端口的进程

`kill -9 <pid>` 杀死该id进程

## 其他命令

`echo 'print("hi!")' > hi.py `创建

`echo 'print("Hello World!")' >> hi.py` 追加

## 常用应用

### ssh

**安装：** `sudo apt install ssh`

**启动服务：** `service ssh start` 默认端口22

**远程连接：** `ssh <用户名>@<服务器地址>`

也可以使用工具：xShell，putty

## 快捷键

`ctrl + alt + t` 打开terminal

`ctrl + alt + l` 锁屏

`ctrl + L` 清屏

