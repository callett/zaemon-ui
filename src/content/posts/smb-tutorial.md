---
title: 安装SMB教程
published: 2025-05-22
updated: 2025-05-22
description: '在 Debian 12 中部署 SMB 文件共享服务'
image: ''
tags: [Bash]
category: 'Tutorial'
draft: false 
---

SMB（Server Message Block）是一个网络文件共享协议，用于在局域网中实现设备间的文件、打印机和其他资源的共享。它最初由IBM开发， 后由微软改进和扩展，是Windows系统的主要文件共享协议，后来也被Linux、macOS等系统支持。

### 1. 安装 Samba 服务
```bash
sudo apt update
sudo apt install samba
```
### 2. 创建 Samba 用户
```bash
sudo useradd myuser
sudo smbpasswd -a myuser
```
该命令会提示你设置 Samba 密码，这是用于通过 Samba 连接共享时的密码，而不是系统登录密码。<br/>
`myuser` 是你要创建的 Samba 用户名，密码用于 SMB 连接时的身份验证。
### 3. 配置Samba共享
```bash
sudo vim /etc/samba/smb.conf
```
在文件中添加共享目录配置
```yml
[SharedFolder]
path = /usr/local    #指定了共享目录在服务器上的物理路径
valid users = myuser #指定只有 myuser 用户能够访问该共享目录
read only = no       #表示共享目录可以进行写入操作
browsable = yes      #表示可以通过文件浏览器查看该共享
```
给用户和文件夹设置正确权限
```bash
sudo chown myuser:myuser /usr/local
sudo chmod 755 /usr/local
```
### 4. 设置防火墙
```bash
sudo ufw allow samba
```
### 5. 重启 Samba 服务
```bash
sudo systemctl restart smbd
```
### 6. 检查 Samba 服务状态
```bash
sudo systemctl status smbd
```
### 7. 通过 Samba 共享访问文件
在 macOS 上，通过访达连接到 Samba 共享：
#### 7.1 打开访达（Finder）
#### 7.2 在菜单栏选择前往->连接服务器，或者按下Cmd+K
#### 7.3 在弹出的框中，输入smb:/<服务器的IP地址>/SharedFolder，例如：
```text
smb://127.0.0.1/SharedFolder
```
#### 7.4 输入 Samba 用户名（如 myuser ）和密码进行连接