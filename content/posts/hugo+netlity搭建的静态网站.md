---
title: "Hugo+netlity搭建的静态网站"
date: 2024-05-29T15:52:45+08:00

---

# 第一步：注册github账号。

# 第二步：注册netlify账号。

# github新建库，名为mywebsite，私人储存库。

# 安装hugo
1. 进入hugogithub官网按照文档下载并安装好。
2. 将GitHub上mywebsite储存库clone下来。
    - git先去官网下载安装配置好name：
    ```bash
    git config --global user.name "John Doe"
    ```
    - email配置：    
    ```bash
    git config --global user.email johndoe@example.com
    ```
    - 生成ssh密钥
    ```bash
    ssh-keygen -t rsa -c
    ```
    - 将生成的ssh公钥添加到GitHub里。
    - 在GitHub设置里。
    - 第二种，拉取私库的方法(token)
    - 在github我的settings中的developer settings选择personal access tokens
    - 选择tokens(classic),新建new tokens。
    - 现在可以克隆库了
    ```bash
    git clone https://输入刚刚生成的tokens@github.com/miaoyehao/mywebsite.git
    ```
    - 记住密码
    ```bash
    git config --global credential.helper store
    ```
    - 拉库
    ```bash
    git pull origin main
    ```
3. 安装主题
    ```bash
    cd mywebsite
    git init
    git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
    ```
