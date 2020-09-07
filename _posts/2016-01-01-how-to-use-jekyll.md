---
layout: post
title: 如何使用Jekyll搭建个人博客
categories:  Tools
tags: Jekyll
---



### 安装和使用Jekyll

第一步：在Mac上安装Jekyll

```bash
sudo gem install bundler jekyll
```

第二步：在Repo文件夹下创建Gemfile，并修改Gemfile为如下两行内容

```shell
bundle init

source "https://rubygems.org"
gem 'github-pages', group: :jekyll_plugins
```

第三步：根据Gemfile安装依赖包


```shell
bundle install
```

第四步：编译个人repo并生成serving

```shell
bundle exec jekyll serve
```

第五步：打开本地网站即可查看，并实时监控网页变化 

http://localhost:4000/




### 参考：

1. [Jekyll Docs](https://jekyllrb.com/docs/)
2. [Setting up your GitHub Pages site locally with Jekyll](https://docs.github.com/en/enterprise/2.14/user/articles/setting-up-your-github-pages-site-locally-with-jekyll#keeping-your-site-up-to-date-with-the-github-pages-gem)