---
title: 开启BBR3教程
published: 2025-05-24
updated: 2025-05-24
description: '教你在 Debian 12 上开启 BBR3 提升网络性能'
image: ''
tags: [Bash]
category: 'Tutorial'
draft: false 
---

BBR（Bottleneck Bandwidth and Round-trip Propagation Time）是一种由Google开发的TCP拥塞控制算法， 用于优化网络性能。 BBR3是BBR的最新版本（截至2024年），相较于BBR1和BBR2，它进一步改进了网络性能优化机制， 尤其在处理丢包率高的 网络环境和RTT（往返时间）变化显著的场景中更具优势。BBR3的作用包括提高网络吞吐量、优化延迟、改善稳定性、减少网络抖动等。

### 1. 安装系统组件
```bash
apt update -y && apt install -y wget gnupg
```
### 2. 注册PGP密钥
```bash
wget -qO - https://dl.xanmod.org/archive.key | gpg --dearmor -o /usr/share/keyrings/xanmod-archive-keyring.gpg --yes
```
### 3. 添加存储库
```bash
echo 'deb [signed-by=/usr/share/keyrings/xanmod-archive-keyring.gpg] http://deb.xanmod.org releases main' | tee /etc/apt/sources.list.d/xanmod-release.list
```
### 4. 查看当前服务器适合的版本
```bash
wget -q https://dl.xanmod.org/check_x86-64_psabi.sh && chmod +x check_x86-64_psabi.sh && ./check_x86-64_psabi.sh
```
### 5. 更新并安装指定内核版本
更老的机型如大西洋
```bash
apt update -y && apt install -y linux-xanmod-x64v1
```
老机型如CC，搬瓦工，RN
```bash
apt update -y && apt install -y linux-xanmod-x64v2
```
大众机型且DD过系统的
```bash
apt update -y && apt install -y linux-xanmod-x64v3
```
新机型莱卡云，谷歌云，微软云，甲骨文云，V.PS，Vultr，DO，Linode等
```bash
apt update -y && apt install -y linux-xanmod-x64v4
```
### 6. 开启 `BBR3`
```text
cat > /etc/sysctl.conf << EOF
net.core.default_qdisc=fq_pie
net.ipv4.tcp_congestion_control=bbr
EOF
```
使配置生效 `sysctl -p`
### 7. 重启系统
```bash
reboot
```
### 8. 查看 `BBR3` 状态
```bash
modinfo tcp_bbr
```