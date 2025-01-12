---
title: "Hugo+netlity搭建的静态网站"
date: 2024-05-29T15:52:45+08:00

---

# 前置条件github账号、netlify账号；然后github新建库，名为mywebsite，最好是私人储存库。

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
4. hugo配置
   ```bash
   baseURL = "网址"
   title = "标题"
   languageCode = "zh-CN"
   theme = "blowfish"
   ```    
5. 撰写文章
   - Hugo要新增文章可以选择在content/posts下新增多个xxx.md的档案，也可以每篇文章一个目录＋index.md。本文採用的是后者作法，以Hugo的术语来说称作page bundle，index.md的作用等同index.html，这样可以方便你整理每篇文章所需的资源。
   - index.md输入以下内容，表示文章属性，即分割线---包围的部分。
   ```bash
    ---
    title: "我的第一篇文章"
    date: 2023-03-25T17:00:00+08:00
    draft: false
    ---
    ![](https://c.tenor.com/x8v1oNUOmg4AAAAd/rickroll-roll.gif)
   ```
6. Hugo一律以Markdown语法撰写文章，可插入HTML、CSS、JavaScript装饰。
7. Hugo要插入图片有很多种方法。上面的例子是把图片放外部图床，再直接贴网址，这样网站储存库就只有文字档案，减少容量。