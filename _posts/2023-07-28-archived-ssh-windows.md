---
layout:     post
title:      "✏️ 配置windows免密码登录的注意事项"
subtitle:   "✏️ Key-based authentication in OpenSSH for Windows"
date:       2022-07-28 23:59:00
author:     "Shicong Liu"
header-img: "img/home-dark.jpg"
catalog: true
mathjax: true
comment: true
hide-in-nav: true
tags:
    - 学习
---



Windows的易用性比起Linux来说确实很差，如果不是部分软件必须使用（在我这里是Microsoft Visio），Windows对我毫无吸引力。近期配置实验室机器反向代理，遇到ssh登陆无法免除密码这种现象。无论windows自己的官方文档[^doc]还是chatgpt提供的建议都无法跳过。由于下半年可能还需要建设香港的实验室基础设施，这里就先准备一下基本流程

### 密钥生成

```bash
ssh-keygen -t rsa -C "<comment>"
```

根据需求起名，最好能和任务对应，否则后面忘记了是谁的key了。passphrase我个人建议不设置，否则有点脱了裤子放屁的意思。

### 公钥处理

通过某些方式将`.pub`结尾的公钥发送到服务器上，并做如下操作

- 进入服务器`~/.ssh/`目录下，将公钥置于其中，复制内容至`authorized_keys`文件内
- 进入服务器`X:\ProgramData\ssh`目录，修改`sshd_config`文件如下

```bash
PubkeyAuthentication yes
```

<mark>并注释掉</mark>最后两行`Match Group administrators`相关的内容

### 刷新SSH Server服务

进入service，重启`OpenSSH SSH Server`服务

### 本地配置

编辑本机`~/.ssh/config`文件如下

```bash
Host <aka.nickname>
    HostName <Your server IP>
    Port <most common 22>
    User <Your server username>
    IdentityFile ~/.ssh/<your private key>
```

---

windows机器实际上多了一步用户组管理，这一条对中国个人用户来说几乎毫无意义。



# 扩展阅读

[^doc]:[Key-based authentication in OpenSSH for Windows](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement)

