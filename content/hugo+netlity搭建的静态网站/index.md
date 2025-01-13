---
title: "Hugo+netlity搭建的静态网站"
date: 2024-05-29T15:52:45+08:00
---

<style>
.apple-divider {
    border: none;
    height: 1px;
    background: #d2d2d7;
    margin: 2.5rem auto;
    max-width: 980px;
}
</style>

<div style="text-align: center;">

#### 准备工作

</div>

##### 前置条件github账号、netlify账号；然后github新建库，名为mywebsite，最好是私人储存库。

<hr class="apple-divider">

#### 安装hugo
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

<hr class="apple-divider">

#### 配置和使用
##### 安装主题
```bash
cd mywebsite
git init
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```
##### hugo配置
```toml
baseURL = "网址"
title = "标题"
languageCode = "zh-CN"
theme = "blowfish"
```    
##### 撰写文章
   - Hugo要新增文章可以选择在content/posts下新增多个xxx.md的档案，也可以每篇文章一个目录＋index.md。本文採用的是后者作法，以Hugo的术语来说称作page bundle，index.md的作用等同index.html，这样可以方便你整理每篇文章所需的资源。
   - index.md输入以下内容，表示文章属性，即分割线---包围的部分。
```yaml
---
title: "我的第一篇文章"
date: 2023-03-25T17:00:00+08:00
draft: false
---
```
    ![](https://c.tenor.com/x8v1oNUOmg4AAAAd/rickroll-roll.gif)
6. Hugo一律以Markdown语法撰写文章，可插入HTML、CSS、JavaScript装饰。
7. Hugo要插入图片有很多种方法。上面的例子是把图片放外部图床，再直接贴网址，这样网站储存库就只有文字档案，减少容量。
8. 本机预览
```bash
hugo server
```
9. 另外介绍Hugo好用的功能：网站根目录一有档案变更，不用停止伺服器，Hugo也会自动重新生成网站。
10. 如果执行hugo指令，就是单纯生成html，你可以此评估网站生成速度，网站根目录下会產生一个public目录，那就是静態网站的「成品」。
11. 將网站原始码推送到Github
```bash
cd mywebsite
#远端Git储存库设定
git remote add origin "网址"
#推送
git add -A
git commit -m "网站更新"
git push -u origin main
```
    脚本
```bash
git add -A
git commit -m "网站更新"
git push
echo -e "\e[93mDeployed to Netlify.\e[0m"
```

<hr class="apple-divider">
#### github|netlify配置##### Netlify设置1. 在Hugo网站根目录新增netlify.toml配置文件```toml# 部署时执行的指令，--minify压缩HTML，--gc自动在建置后刪除快取档案[build]
publish = "public"
command = "hugo --gc --minify"

# 指定Hugo版本，应与本机安装的Hugo版本一致
[build.environment]
HUGO_VERSION = "0.121.1"
```
2. 登陆netlify，点击Add a site - import an existing project from a git repository选择我们的网站库。
3. 选择site setting，Domains management中的Domains添加自己的域名
4. 添加Netlify DNS到你的域名托管。