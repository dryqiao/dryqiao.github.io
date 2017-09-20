---
title: GithubPages教程
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

    ![git](/public/img/git1.png)
    
    * push 到仓库，

    ![git](/public/img/git2.png)
    * 浏览器上输入`<username>.github.io`，完成

    ![git](/public/img/git3.png)

---

## 二、 Github Pages个人博客绑定域名

刚才的域名`<username>.github.io`显然不好记，不符合我们程序员能简则简的理念。那我们怎么办呢？

1、首先，我们需要一个域名，没有的话去申请一个，例如[万网](https://wanwang.aliyun.com/),github推荐使用www的二级域名。

2、ping一下自己的github pages站点，获取到IP地址。

3、域名解析，输入刚才得到的IP地址。

4、在项目根目录创建`CNAME`文件,里面输入刚才注册的域名`www.xxx.xxx`，保存并push到github。

5、浏览器访问`www.xxx.xxx`，页面会自动打开刚才生成的github pages站点。完成！

![git](/public/img/git4.png)


