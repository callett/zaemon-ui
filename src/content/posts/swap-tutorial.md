---
title: 新增Swap教程
published: 2025-05-23
updated: 2025-05-23
description: '在 Debian 12 上添加并启用 Swap 虚拟内存以提升系统稳定性'
image: ''
tags: [Bash]
category: 'Tutorial'
draft: false 
---

Swap是Linux系统中的一种虚拟内存机制。当系统的物理内存不足时，Swap空间会被用作内存的补充， 将不常用的内存页存储到硬盘中， 释放物理内存以供当前活跃的进程使用。Swap的作用包括缓解内存不足、提高系统稳定性、 支持内存分页、降低系统开销等。

### 1. 新增一个 swap 文件
```bash
dd if=/dev/zero of=/opt/swapfile bs=1M count=1024
```
这个命令在 `/root` 目录下生成了一个文件，名为 `swapfile`，大小为1G
### 2. 设置交换分区文件
将第一步中创建的文件指定为 `swap` 分区的文件，使用 `mkswap` 指令建立交换分区
```bash
mkswap /opt/swapfile
```
### 3. 激活交换分区文件，使用 swapan 命令激活交换空间
```bash
swapon /opt/swapfile
```
到了这一步 `swap` 分区已经生效了，但是重启机器后就会失效，所以我们下一步就是让它开机自动挂载 `swap` 分区
### 4. 让 swap 分区系统开机时自启用
编辑 `vim /etc/fstab` 文件，新增一行
```bash
/opt/swapfile swap swap defaults 0 0
```
### 5. 让 swap 生效
```bash
sysctl -p
```
<br><br/>

:::warning
注意事项
:::
```text
• Swap的读写速度远低于物理内存，因此频繁使用Swap空间会显著降低性能
• 对于内存资源充足的系统，可以选择减少Swap的使用
```