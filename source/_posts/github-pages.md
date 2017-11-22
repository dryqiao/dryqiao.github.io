---
title: GithubPages教程

date: 2017/10/21 19:21:13
---
# GithubPages教程

>* github pages 个人博客搭建
>* Github Pages个人博客绑定域名
>* jekyll入门
---
## 一、github pages 个人博客搭建
1. github pages是什么

* [github pages官网介绍](https://pages.github.com/)


> github paged是github提供的可以免费托管用户静态页面的站点，它提供模板站内生成网站，用户也可以自行编写网页，上传。

2. github pages怎么用

* [官网教程](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/)

    * 新建一个新项目

    * 项目名称为 `<username>.github.io`

    * 点击`create repository`

    * git clone 到本地

    * 新建index.html,输入`hello world`

    ![git](http://owicv5j2l.bkt.clouddn.com/git1.png)
    
    * push 到仓库，

    ![git](http://owicv5j2l.bkt.clouddn.com/git2.png)
    * 浏览器上输入`<username>.github.io`，完成

    ![git](http://owicv5j2l.bkt.clouddn.com/git3.png)

---

## 二、 Github Pages个人博客绑定域名

刚才的域名`<username>.github.io`显然不好记，不符合我们程序员能简则简的理念。那我们怎么办呢？

1、首先，我们需要一个域名，没有的话去申请一个，例如[万网](https://wanwang.aliyun.com/),github推荐使用www的二级域名。

2、ping一下自己的github pages站点，获取到IP地址。

3、域名解析，输入刚才得到的IP地址。

4、在项目根目录创建`CNAME`文件,里面输入刚才注册的域名`www.xxx.xxx`，保存并push到github。

5、浏览器访问`www.xxx.xxx`，页面会自动打开刚才生成的github pages站点。完成！

![git](http://owicv5j2l.bkt.clouddn.com/git4.png)


## 三、jekyll入门
Jekyll是一个静态站点生成器，它会根据网页源码生成静态文件。
在本地编写符合jekyll规范的源码，上传到github，由github生成博客并托管。

优点：

1. 免费，无限制
2. 由git托管，享受版本控制，不担心文章丢失

缺点：
1. 要懂git。
2. 最后生成的是静态网页，想增加动态功能必须导入外部服务，如站内搜索、评论。
3. 没有数据库，每次生成需要遍历所有文件，不适合大型博客

实例：

1. 在刚才的文件夹内，删掉index.html,创建以下文件

    `_config.yml`：jekyll的配置文件

    `_layouts` ： 存放模板的文件夹,创建default.html,写入以下内容：
    ```
　　<!DOCTYPE html>
　　<html>
　　<head>
　　　　<meta http-equiv="content-type" content="text/html; charset=utf-8" />
　　　　<title>{{ page.title }}</title>
　　</head>
　　<body>

　　　　{{ content }}

　　</body>
　　</html>
    ```
    `_posts` ： 存放文章，支持html和md格式

    `index.html` : 首页，写入以下内容：

    ```
    ---
    layout: default
    title: 我的Blog
    ---

    <h2>{{ page.title }}</h2>
　　<p>最新文章</p>
　　<ul>
　　　　{% for post in site.posts %}
　　　　　　<li>{{ post.date | date_to_string }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
　　　　{% endfor %}
　　</ul>
    ```
    它已yaml文件头表示首页调用的default模板，title是`我的blog`,

2. 开始写博客，博客名要为`yyyy-mm-dd-文章标题-后缀`,创建`2017-09-20-helloWorld.md`，写入下面内容
    ```
    ---
    layout: default
    title: 你好，世界
    ---
    # {{ page.title }}

    ## 我的第一篇文章

    #### Hello World!
    ```
3. 提交到仓库
    ```
    git add .
    git commit -m""
    git push origin master
    ```

4. 访问`username.github.io`,就可以看到自己写的博客了，基本功能介绍到这，想博客变得更好看的话，可以自己写代码，也可以去官网fork别人的主题。