---
title: "Hugo+netlity搭建的静态网站"
date: 2023-06-27T15:52:45+08:00

---

# 第一步：注册github账号。

# 第二步：注册netlify账号。

# github新建库，名为mywebsite，私人储存库。

# 安装hugo
### 一、进入hugogithub官网按照文档下载并安装好。
### 二、将GitHub上mywebsite储存库clone下来。
#### （1）git先去官网下载安装配置好name：
    git config --global user.name "John Doe"
#### （2）email配置：    
    git config --global user.email johndoe@example.com
#### （3）生成ssh密钥
    ssh-keygen -t rsa -c
#### （4）将生成的ssh公钥添加到GitHub里。
##### 在GitHub设置里。
#### 第二种，拉取私库的方法(token)
#### 在github我的settings中的developer settings选择personal access tokens
##### 选择tokens(classic),新建new tokens。
##### 现在可以克隆库了
    git clone https://输入刚刚生成的tokens@github.com/miaoyehao/mywebsite.git
##### 记住密码
    git config --global credential.helper store
##### 拉库
    git pull origin main
### 三、安装主题
    cd mywebsite
    git init
    git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
    
