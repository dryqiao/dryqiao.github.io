---
title: HEXO+GITHUB 零前端基础建站教程

date: 2017/9/30 10:16:03
categories: 前端
---

# HEXO+GITHUB 零前端基础建站教程

技术栈：

* hexo
* git

环境:

* node
* git

## 一、github pages以及域名的搭建

见上一篇博客

## 二、Hexo入门

Hexo是一个快速、简洁且高效的博客框架，与上一篇博客讲到的jekyll类似。那为什么选择Hexo呢？

看脸233333。。官网的对比，自己选择吧

[Hexo](https://hexo.io/)  
[jekyll](http://jekyll.com.cn/)

正式开始

1. 安装hexo

`npm install hexo-cli -g`

2. 初始化项目

选择一个路径，执行以下命令创建项目，博客名称自己想

```
 hexo init <blog>
 cd blog
 hexo init
 npm install```


3. 生成页面，本地起点服务

```
hexo generate
hexo server
```
执行完后会发现项目目录下多了一个public目录，这个目录里就是框架自动生成的静态文件，博客所有代码，文章都在这里

上述指令可以用缩写
```
hexo g
hexo s
```

![hexog](http://owicv5j2l.bkt.clouddn.com/hexo_g.png)

默认端口号为4000，生成成功后，有时候会有冲突，用以下命令来修改端口号
`hexo s -p 5000`
![hexos](http://owicv5j2l.bkt.clouddn.com/hexo_s.png)

OK,这个时候博客已经在本地搭建完成了

来看一下这几个文件目录
_config.yml //博客的配置，
source //文章放在source/_posts文件夹里，md格式

4. 发表第一篇博客
在source/_posts里新建 hello.md

输入以下内容，每篇文章的基础信息都写在“---”里
```
---
title: Hello World
---

HELLO WORLD !!!
```
保存，执行之前的两条命令
```
hexo g
hexo s -p 5000
```
OK,打开http://localhost:5000/看自己的文章吧


## 三、发布到github pages站点上

1. 安装hexo自动部署工具

`npm instal lhexo-deployer-git  --save`

2. 配置github信息
打开_config.yml，这个文件是整个项目的配置文件,配置不详细介绍了，主要修改以下配置，

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: 你自己的仓库地址
  branch: master//分支

```

3. 发布

```
hexo g
hexo deployer //可以用缩写d
```

打开你的github pages站点，<username.github.io>
可以看到刚才所写的'hello world'文章

进入自己的github仓库里，可以看到项目目录为：

![hexod](http://owicv5j2l.bkt.clouddn.com/hexo_d.png)

是不是很眼熟，没错，hexo d就是把生成的public静态文件夹的内容git push到仓库里。

那问题来了，hexo d会重新push一遍，把hexo的项目文件冲掉，那我们怎么提交hexo的项目文件呢，不可能每次都在这台电脑上写文章吧。

git的强大又一次体现出来了，我们可以利用分支。

hexo的项目内容放在hexo分支上，当然这个分支名字你可以随便取。
hexo deployer生成的内容放在默认的master分支上。
这样，我们就可以随时随地拿到代码，并且写文章了。


写博客流程：
1、 git获取远程仓库的hexo分支代码
2、 写博客文章
3、 源代码git push到 hexo分支
4、 hexo d自动部署
5、 完成。


