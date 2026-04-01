---
title: VSCode 远程服务器连接指南（堡垒机方案）
description: 使用 SSH + 堡垒机 + VSCode Remote 实现内网服务器远程开发
outline: deep
lastUpdated: true
---

# 🔐 VSCode 远程服务器连接指南（堡垒机方案）

> 适用于：通过堡垒机访问内网服务器，并使用 VSCode Remote 进行开发

---

## 🧭 一、整体流程

```mermaid
graph LR
A[本机] --> B[堡垒机]
B --> C[目标服务器]
C --> D[VSCode Remote]
```
## 🔑 二、本机生成 SSH 密钥
``` bash
ssh-keygen -t dsa
```

默认生成路径：

```bash
~/.ssh/id_dsa
~/.ssh/id_dsa.pub
```

:::tip 说明

id_dsa：私钥（⚠️ 严禁泄露）
id_dsa.pub：公钥（用于上传）
:::

## 🌐 三、登录堡垒机

访问地址：
```bash
https://blj.supconit.com/dashboard
```


## ⚙️ 四、堡垒机配置
```bash
1. 登录堡垒机
2. 进入个人信息页面
3. 配置 SSH 密钥：
    公钥 → 填入 id_dsa.pub
    私钥 → 填入 id_dsa
```
:::warning 安全提示
私钥属于敏感信息，请确保堡垒机平台可信
:::

## 🖥️ 五、目标服务器配置

将本机公钥写入目标服务器：

### 方法一（推荐）
```bash 
vim ~/.ssh/authorized_keys
```

粘贴 id_dsa.pub 内容

### 方法二（命令行）
```bash
echo -n >> ~/.ssh/authorized_keys
cat id_dsa.pub >> ~/.ssh/authorized_keys
```
设置权限（必须）
```bash
chmod 600 ~/.ssh/authorized_keys
```
:::warning 
权限不正确会导致 SSH 登录失败
:::


## 🧩 六、VSCode 配置

1️⃣ 安装插件

安装：
- Remote - SSH

2️⃣ 配置 SSH

编辑文件：

```bash
~/.ssh/config
```

添加：
```bash
Host target-server
    HostName 192.168.4.20
    User scv
    ProxyCommand ssh 20210082@172.16.3.74 -p 60022 -i ~/.ssh/id_dsa -W %h:%p
```
### 📌 参数说明
|参数	| 说明 |
|------| -----|
|Host	|本地别名|
|HostName|	目标服务器 IP|
|User|	登录用户|
|ProxyCommand	|通过堡垒机跳转|



## 🚀 七、VSCode 连接
打开命令面板：
```bash
Ctrl + Shift + P
```
输入：
```bash
Remote-SSH: Connect to Host
```
选择：
```bash
target-server
```